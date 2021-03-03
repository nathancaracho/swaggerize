<template>
  <div class="spinner-Wrapper">
    <svg class="spinner" viewBox="0 0 50 50">
        <circle class="path" cx="25" cy="25" r="20" fill="none" stroke-width="5" stroke="#BBB"></circle>
    </svg>
  </div>
</template>

<script>
export default {

}
</script>

<style >
  .spinner-Wrapper{
    text-align: center;
  }
  .spinner{
    margin: auto;
    animation: rotate 2s linear infinite;
    z-index: 2;
    width: 50px;
    height: 50px;
  }
  .spinner > .path{
   
    stroke-linecap: round;
    animation: dash 1.5s ease-in-out infinite;
  }

  @keyframes rotate {
  100% {
    transform: rotate(360deg);
  }
  }

  @keyframes dash {
    0% {
      stroke-dasharray: 1, 150;
      stroke-dashoffset: 0;
    }
    50% {
      stroke-dasharray: 90, 150;
      stroke-dashoffset: -35;
    }
    100% {
      stroke-dasharray: 90, 150;
      stroke-dashoffset: -124;
    }
  }

</style>
//based on
//https://codepen.io/supah/pen/BjYLdW