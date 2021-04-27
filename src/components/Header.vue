<template>
  <div class="header">
    <span> Fancy quiz app </span>
    <span> Score: {{correctCtr}}/{{totalCtr}} </span>
  </div>
</template>

<script>

export default {
  props: [
    'correctCtr',
    'totalCtr'
  ]
}

</script>


<style>
.header {
  display: flex;
  flex-direction: column;
  align-items: flex-start;
}
</style>