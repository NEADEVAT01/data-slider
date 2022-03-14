<template>
  <div class="main">
    <div class="row">
      <div class="col-xl-1 col-sm-12 my-auto mt-1 px-0 ms-2">
        <button class="d-block ms-4 mb-1 " v-on:click="isYear=true" v-bind:class="{ button_pressed: isYear }"
          absorb=true>Все года</button>
        <button class="d-block ms-4" v-on:click="isYear=false"
          v-bind:class="{ button_pressed: !isYear }">Месяца</button>
      </div>
      <div class="col-xl-10 col-sm-11 ps-5 mt-3">
        <vue-slider v-model="YearValue" :tooltip-placement="['top', 'bottom']" v-show='isYear' :max-range=2
          tooltip='always' :processStyle="{height: '6px', margin: '2px'}" marks :min=YearMin :max=YearMax :dotSize=20
          :height=10 @change="calculateMonths(),calculateDate() ">
          <template #tooltip="{ index }">
            <div v-if="index === 0" :class="['custom-tooltip']">{{calculatedDate[0]}}</div>
            <div v-else :class="['custom-tooltip']">{{calculatedDate[1]}}</div>
          </template>
          <template v-slot:step="">
            <div></div>
          </template>
          <template v-slot:label="{label}">
            <div :class="['vue-slider-mark-label', 'custom-label']">{{ label }}</div>
          </template>
          <template v-slot:dot="">
            <div :class="['custom-dot']"></div>
          </template>
        </vue-slider>
        <vue-slider v-model="MonthValue" :tooltip-placement="['top', 'bottom']" v-show='!isYear' :dotSize=20 :height=10
          tooltip='always' :data="calculatedMonths" :data-value="'id'" @change="calculateDate()">
          <template #tooltip="{ index }">
            <div v-if="index === 0" :class="['custom-tooltip']">{{calculatedDate[0]}}</div>
            <div v-else :class="['custom-tooltip']">{{calculatedDate[1]}}</div>
          </template>
          <template v-slot:step="">
            <div></div>
          </template>
          <template v-slot:label="{label,value}">
            <div v-if="value % 12 === 0" :class="['vue-slider-mark-label', 'custom-label bold']">{{ label }}</div>
            <div v-else :class="['vue-slider-mark-label', 'custom-label sizible']">{{ label }}</div>
          </template>
          <template v-slot:dot="">
            <div :class="['custom-dot']"></div>
          </template>
        </vue-slider>
      </div>
    </div>
  </div>
</template>
<script>
  import VueSlider from 'vue-slider-component' //Default slider component
  import 'vue-slider-component/theme/default.css' //Deafaul slider styles
  export default {
    components: {
      VueSlider
    },
    name: 'Main',
    data() {
      return {
        isYear: true, //Slider switch check
        YearValue: [2019, 2021], //Value year slider    
        MonthValue: [0, 84], //Value month slider     
        YearMin: 2014, // Minimal year   
        YearMax: 2021, // Maximal year          
        calculatedDate: ["Январь 2001", "Январь 2022"], //Tooltip Date
        calculatedMonths: [], //Data for Month slider
        Month: [{ //Array of Month to work with
          label: 'янв',
          name: 'Январь'
        }, {
          label: 'фев',
          name: 'Февраль'
        }, {
          label: 'мар',
          name: 'Март'
        }, {
          label: 'апр',
          name: 'Апрель'
        }, {
          label: 'май',
          name: 'Май'
        }, {
          label: 'июн',
          name: 'Июнь'
        }, {
          label: 'июл',
          name: 'Июль'
        }, {
          label: 'авг',
          name: 'Август'
        }, {
          label: 'сен',
          name: 'Сентябрь'
        }, {
          label: 'окт',
          name: 'Октябрь'
        }, {
          label: 'ноя',
          name: 'Ноябрь'
        }, {
          label: 'дек',
          name: 'Декабрь'
        }, ],
      }
    },
    beforeMount: function () {
      this.calculateMonths()
      this.calculateDate()
    },
    methods: {
      calculateDate: function () { //Visualising data for tooltips  
        this.calculatedDate[0] = this.calculatedMonths[this.MonthValue[0]].name + ' ' + this.calculatedMonths[this
          .MonthValue[0] - (this.MonthValue[0] % 12)].label
        this.calculatedDate[1] = this.calculatedMonths[this.MonthValue[1]].name + ' ' + this.calculatedMonths[this
          .MonthValue[1] - (this.MonthValue[1] % 12)].label
      },
      calculateMonths: function () { //Calculating data for mouth slider
        this.calculatedMonths = []
        let $YearSpread = this.YearValue[1] - this.YearValue[0]
        let $count = $YearSpread
        let $id = 0
        while ($count != -1) {
          for (let $i = 0; $i < 12; $i++) {
            if ($i == 0) {
              this.calculatedMonths[$id] = {
                label: this.YearValue[1] - $count,
                name: this.Month[$i]['name'],
                id: $id
              }
            } else {
              this.calculatedMonths[$id] = {
                label: this.Month[$i]['label'],
                name: this.Month[$i]['name'],
                id: $id
              }
            }
            $id++
          }
          $count--
        }
        this.MonthValue[0] = 0
        this.MonthValue[1] = $id - 1
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
    font-size: 0.8vw !important;
    display: block !important;
  }

  .bold {
    color: #333333;
  }

  @import '~vue-slider-component/lib/theme/default.scss';
  @import url('https://fonts.googleapis.com/css2?family=Montserrat:wght@500&display=swap');
</style>