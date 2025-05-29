<!-- CardGenerator.vue -->
<template>
  <div>
    <div>
      <label for="csvInput" style="display: block">Select CSV File</label>

      <input @change="handleFileChange" class="p-2 my-1" id="csvInput" name="file" type="file" />
      <div class="row" style="align-items: center;">
      <input placeholder="enter start" id="start" class="col-md-3 form-control" v-model="start" />
      <input placeholder="enter end" id="end" class="col-md-3 mx-md-1 my-1 my-md-0 form-control" v-model="end" />
      <input
        type="button"
        class="btn btn-primary"
        value="hide buttons"
        @click="hideElements"
      />
      <input
        type="button"
        class="btn btn-primary m-2"
        value="Get pdf"
        @click="printDocument"
      />
      </div>
    </div>
    <div class="text-center mt-2" style="width: 1090px">
      <div id="divToPrint" style="height: 1000%; overflow: hidden">
        <template v-for="(item, index) in slicedData" :key="index">
          <div v-if="(index + 1) % 24 === 0" class="pagebreak"></div>
          <Cardcac v-else :Description="item['Description']" :Upc="item['Upc']" :FR_Code="item['SM_code']" 
          :Naira_price="item['Naira_price']" :Selling_Price="item['Selling_Price']" :pageNumber="index" />
        </template>
      </div>
    </div>
  </div>
</template>

<script>
import Papa from 'papaparse';
import Cardcac from './Cardcac.vue';
import html2canvas from "html2canvas";
import jsPDF from "jspdf";
export default {
  name: 'CardGenerator',
  components: { Cardcac },
  data() {
    return {
      count: 0,
      csvdata: [],
      allowedExtensions: ['csv'],
      error: '',
      start: null,
      end: null,
      file: null
    };
  },
  computed: {
    slicedData() {
      if (this.start != null && this.end != null) {
        return this.csvdata.slice(this.start, this.end);
      }
      return this.csvdata;
    }
  },
  methods: {
    handleFileChange(e) {
      this.error = '';
      const inputFile = e.target.files[0];
      if (!inputFile) return;

      const fileExtension = inputFile.name.split('.').pop().toLowerCase();
      if (!this.allowedExtensions.includes(fileExtension)) {
        this.error = 'Please input a csv file';
        return;
      }

      this.file = inputFile;
      this.handleParse(inputFile);
    },
    handleParse(file) {
      if (!file) return alert('Enter a valid file');

      const reader = new FileReader();
      reader.onload = ({ target }) => {
        const csv = Papa.parse(target.result, {
          header: true
        });
         console.log(this.csvdata)
        this.csvdata = [...csv?.data || []];
        console.log(this.csvdata)
      };
      reader.readAsText(file);
    },
    hideElements() {
      document.querySelector('input').style.display = 'none';
      document.querySelector('label').style.display = 'none';
      event.target.parentElement.style.display = 'none';
    },
  printDocument() {
  const input = document.getElementById("divToPrint");
  if (!input) return;
  html2canvas(input, {
    scale: 2,
    useCORS: true,
    scrollX: 0,
    scrollY: -window.scrollY,
  }).then((canvas) => {
    const imgData = canvas.toDataURL("image/png");
    const pdf = new jsPDF({
      orientation: "portrait",
      unit: "pt",
      format: "a4",
    });
    const imgProps = pdf.getImageProperties(imgData);
    const pdfWidth = pdf.internal.pageSize.getWidth();
    const pdfHeight = (imgProps.height * pdfWidth) / imgProps.width;
    pdf.addImage(imgData, "PNG", 0, 0, pdfWidth, pdfHeight);
    pdf.save("smcards.pdf");
  });
  }
  }
};

</script>

<style scoped>
/* Scoped styles here */
</style>
