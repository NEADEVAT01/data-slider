<template>
  <div class="main">
    <div class="row">
      <div class="col-xl-1 col-sm-12 my-auto mt-1 px-0 ms-2">
        <button class="d-block ms-4 mb-1 " v-on:click="isYear=true" v-bind:class="{ button_pressed: isYear }"
          absorb=true>Все года</button>
        <button class="d-block ms-4" v-on:click="isYear=false"
          v-bind:class="{ button_pressed: !isYear }">Месяца</button>
      </div>
      <Sliders :isYear=this.isYear />
    </div>
  </div>
</template>
<script>
  import 'vue-slider-component/theme/default.css' //Deafaul slider styles  
  import Sliders from './Sliders.vue'
  export default {
    components: {
      Sliders
    },
    name: 'Main',
    data() {
      return {
        isYear: true //Slider switch check    
      }
    }
  }
</script>

<style lang="scss">
  .main {
    font-family: 'Montserrat', sans-serif;
    font-weight: 600;
    padding: 5.5em 0;
    text-align: center;
  }

  .row button {
    padding: 0;
    border: none;
    font: inherit;
    font-size: 14px;
    color: rgba(1, 103, 179, 1);
    opacity: .5;
    background-color: transparent;
    border-bottom: 1px solid #D3DCF1;
  }

  .button_pressed {
    opacity: 1 !important;
    border: none !important;
  }

  .custom-dot {
    width: 100%;
    height: 100%;
    border-radius: 199px;
    background: #FFFFFF;
    border: 5px solid #5CADEA;
    box-sizing: border-box;
    cursor: pointer;
    box-shadow: 0px 3px 5px rgba(31, 85, 132, 0.2);
    transition: .5s;
  }

  .custom-dot:hover {
    transform: scale(1.5);
  }

  .custom-tooltip {
    box-shadow: 0px 5px 15px rgba(14, 88, 143, 0.2);
    background: #FFFFFF;
    padding: 0.2em 1em 0.2em 1em;
    border-radius: 15px;
    font-size: 18px;
    color: #0167B3;
  }

  .custom-label {
    color: #999999;
    font-size: 18px !important;
    display: block !important;
  }

  @media (max-width: 1300px) {
    .custom-label-month {
      writing-mode: vertical-lr;
    }
  }

  .bold {
    color: #333333;
  }

  @import '~vue-slider-component/lib/theme/default.scss';
  @import url('https://fonts.googleapis.com/css2?family=Montserrat:wght@500&display=swap');
</style>