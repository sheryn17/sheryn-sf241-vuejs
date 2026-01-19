<template>
  <div class="pears">
    <h2>{{ name }}</h2>
    <p>{{ message }}</p>
  </div>
</template>

<script>
export default {
  data() {
    return {
      name: 'Pears, Dalandan, Papaya',
      message: 'I like pears, dalandan, and papaya'
    }
  }
};
</script>

<style>
  .pears {
    border: dashed black 1px;
    display: inline-block;
    margin: 10px;
    padding: 10px;
    background-color: red;
  }
</style>