<template>
   <div @click="copy" class="copy">
      <p><b>Copy base url</b>: {{ urlBase }}</p>
    </div>
</template>

<script>
export default {
  props:{
    urlBase: String
  },
    methods: {
    copy() {
      navigator.clipboard.writeText(this.urlBase);
    },
  }
}
</script>
<style>
  .copy > *{
    position: relative;
    cursor: pointer;
    font-weight: 500;
    text-decoration: none;
  }
  .copy:hover {
    text-decoration: underline;
  }
</style>