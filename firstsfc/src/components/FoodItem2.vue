<template>
  <div class="papaya">
    <h2>{{ name }}</h2>
    <p>{{ message }}</p>
  </div>
</template>

<script>
export default {
  data() {
    return {
      name: 'Papaya and Melon',
      message: 'I like pears, dalandan, papaya and melon'
    }
  }
};
</script>

<style>
  .papaya {
    border: dashed black 1px;
    display: inline-block;
    margin: 10px;
    padding: 10px;
    background-color: green;
  }
</style>