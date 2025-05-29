<template>
  <svg ref="barcode"></svg>
</template>

<script>
import JsBarcode from 'jsbarcode';
import { onMounted, ref, watch } from 'vue';

export default {
  name: 'Barcode',
  props: {
    value: String
  },
  setup(props) {
    const barcode = ref(null);

    const generateBarcode = () => {
      if (barcode.value && props.value) {
        JsBarcode(barcode.value, props.value, { width: 2, height: 40 });
      }
    };

    onMounted(generateBarcode);
    watch(() => props.value, generateBarcode);

    return { barcode };
  }
};
</script>
