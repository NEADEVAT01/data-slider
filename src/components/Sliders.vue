<template>  
      <div class="col-xl-10 col-sm-11 ps-5 mt-3">
        <!-- Year Slider -->
        <vue-slider v-model="yearValue" ref='yearSlider' :tooltip-placement="['top', 'bottom']" v-show='isYear' :max-range=2
          tooltip='always' :processStyle="{height: '6px', margin: '2px'}" marks :min=yearMin :max=yearMax :dotSize=20
          :height=10 @change="calculateMonths(),calculateDate()">
          <template #tooltip="{ index }">
            <div v-if="index == 0" :class="['custom-tooltip']">{{calculatedDate[0]}}</div>
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
        <!---->
        <!-- Month Slider -->
        <vue-slider v-model="monthValue" :tooltip-placement="['top', 'bottom']" v-show='!isYear' :dotSize=20 :height=10
          tooltip='always' :data="calculatedMonths" :data-value="'id'" @change="calculateDate(), changeYear()">
          <template #tooltip="{ index }">
            <div v-if="index == 0" :class="['custom-tooltip']">{{calculatedDate[0]}}</div>
            <div v-else :class="['custom-tooltip']">{{calculatedDate[1]}}</div>
          </template>
          <template v-slot:step="">
            <div></div>
          </template>
          <template v-slot:label="{label,value}">
            <div v-if="value % 12 == 0" :class="['vue-slider-mark-label', 'custom-label bold custom-label-month']">{{ label }}</div>
            <div v-else :class="['vue-slider-mark-label', 'custom-label custom-label-month']">{{ label }}</div>
          </template>
          <template v-slot:dot="">
            <div :class="['custom-dot']"></div>
          </template>
        </vue-slider>
         <!---->
      </div>    
</template>
<script>
  import VueSlider from 'vue-slider-component' //Default slider component  
  export default {
    components: {
      VueSlider
    },
    name: 'Sliders',
    props: {
      isYear: Boolean
    },
    data() {
      return {        
        yearValue: [2019, 2021], //Value year slider    
        monthValue: [0, 84], //Value month slider     
        yearMin: 2014, // Minimal year   
        yearMax: 2021, // Maximal year          
        calculatedDate: ["Январь 2001", "Январь 2022"], //Tooltip Date
        calculatedMonths: [], //Data for month slider
        month: [{ //Array of month to work with
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
      changeYear: function () {          
          this.yearValue[0] = this.calculatedMonths[this.monthValue[0] - (this.monthValue[0] % 12)].label          
          this.yearValue[1] = this.calculatedMonths[this.monthValue[1] - (this.monthValue[1] % 12)].label
          this.$refs.yearSlider.setValue(this.yearValue)          
      },
      calculateDate: function () { //Visualising data for tooltips  
        this.calculatedDate[0] = this.calculatedMonths[this.monthValue[0]].name + ' ' + this.calculatedMonths[this
          .monthValue[0] - (this.monthValue[0] % 12)].label
        this.calculatedDate[1] = this.calculatedMonths[this.monthValue[1]].name + ' ' + this.calculatedMonths[this
          .monthValue[1] - (this.monthValue[1] % 12)].label
      },
      calculateMonths: function () { //Calculating data for mouth slider
        this.calculatedMonths = []
        const YearSpread = this.yearValue[1] - this.yearValue[0]
        let count = YearSpread
        let id = 0
        while (count != -1) {
          for (let i = 0; i < 12; i++) {
            if (i == 0) {
              this.calculatedMonths[id] = {
                label: this.yearValue[1] - count,
                name: this.month[i]['name'],
                id: id
              }
            } else {
              this.calculatedMonths[id] = {
                label: this.month[i]['label'],
                name: this.month[i]['name'],
                id: id
              }
            }
            id++
          }
          count--
        }
        this.monthValue[0] = this.monthValue[0] % 12
        this.monthValue[1] = id - 1
      }
    }
  }
</script>