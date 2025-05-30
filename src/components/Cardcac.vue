<!-- Cardcac.vue -->
<template>
  <div
    class="col-4 border"
    style="width: 359.33px; float: left; padding: 15px;"
  >
    <div class="" style="height: 85%; width: 90%">
      <div class="my-2 mx-auto" style="height: 100px; width: 200px">
        <img src="/images/asset2.jpg" style="width: 100%; height: auto" />
      </div>
      <div class="d-flex flex-column">
        <div class="mt-2">
          <Barcode :value="Upc" :width="2" :height="40" />
        </div>
        <div
          class=""
          style="
            width: 100%;
            font-size: 14px;
            font-weight: bolder;
            text-overflow: ellipsis;
          "
        >
          {{ Description }}
        </div>
        <div>
          <span style="height: 30px; font-size: 14px; font-weight: bolder"
            >{{ Selling_Price ?? 0.0 }} ESPEES</span
          >
        </div>
        <div>
          <span style="height: 30px; font-size: 14px; font-weight: bolder"
            >{{ Naira_price }}</span
          >
        </div>
        <!-- <div>{{ pageNumber }}</div> -->
      </div>
    </div>
  </div>
</template>

<script>
import Barcode from './barcode.vue';

export default {
  name: 'Cardcac',
  components: {
    Barcode
  },
  props: {
    Description: String,
    FR_Code: String,
    Upc: String,
    Naira_price: [String, Number],
    Selling_Price: [String, Number],
    Cost_Price: [String, Number],
    pageNumber: Number
  }
};
</script>

<style scoped>
/* Scoped styles here */
</style>