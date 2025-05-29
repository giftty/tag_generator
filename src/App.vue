<script setup>
  import CardGenerator from './components/Cardgenerator.vue'
</script>

<template>
  
  <CardGenerator />

  <!-- <main>
    <TheWelcome />
  </main> -->
</template>

<style scoped>
@media print {
  .pagebreak{ page-break-before: always;}
}
input::placeholder {
	font-size: 15px;
}
div{
 font-size: 16px;
}

.file-container{
 border: 2px dashed rgba(22, 15, 15, 0.08);
 margin: var(--size8) 0;
height: 104px;
border-radius: 8px;
font-size: 12px !important;
line-height: 16px;
transition: all 1s ease-out;
}
.pointer-cursor{
 cursor: pointer;
}
</style>
