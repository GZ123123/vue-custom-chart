<script setup>
import { use } from "echarts/core";
import { RadarChart } from "echarts/charts";
import { SVGRenderer } from 'echarts/renderers'
import {
  PolarComponent,
  TitleComponent,
  TooltipComponent
} from "echarts/components";
import { provide, shallowRef, ref, computed } from "vue";
import VChart, { INIT_OPTIONS_KEY } from "vue-echarts";
import VExample from './Example.vue'

use([SVGRenderer, RadarChart, PolarComponent, TitleComponent, TooltipComponent]);

const initOptions = computed(() => ({
  renderer: "svg"
}));

provide(INIT_OPTIONS_KEY, initOptions);

const scores = ref([
    { name: "1.待遇面の満足度", max: 5, value: 5 },
    { name: "2.メンバーの士気", max: 5, value: 5 },
    { name: "3.風通しの良さ", max: 5, value: 5 },
    { name: "4.社員の相互尊重", max: 5, value: 5 },
    { name: "5.成長環境", max: 5, value: 5 },
    { name: "6.人材の長期育成", max: 5, value: 5 },
    { name: "7.人事評価の適性感", max: 5, value: 5 },
  ]);

  const metrics = computed(() => {
    return scores.value.map(({ name }) => name);
  });

  function getRadarData(activeIndex) {
    return {
      animation: false,
      tooltip: false,
      grid: false,
      radar: {
        axisLine: {
          lineStyle: {
            color: '#E5E5E5',
            width: 1
          }
        },
        axisName: {
          shape: 'circle',
          formatter: function (value, indicator) {
            return `{image|}\n{name|${value}}\n{value|${indicator.value}}`;
          },
          color: '#000',
          textAlign: 'center',
          rich: {
            image: {
              height: 25,
              width: 25,
              backgroundColor: {
                image: 'https://echarts.apache.org/examples/data/asset/img/weather/cloudy_128.png'
              },
              align: 'center'
            },
            value: {
              color: '#4B2396',
              fontWeight: 700,
              lineHeight: 22,
              fontSize: 16,
              align: 'center'
            }
          }
        },
        
        splitArea: false,
        splitLine: {
          lineStyle: {
            color: '#E5E5E5',
            width: 1
          }
        },
        splitNumber: 1,
        indicator: scores.value.map(({ name, max, value }, index) => {
          return { value, name, max };
        })
      },
      series: [
        {
          name: "Value",
          type: "radar",
          data: [{ value: scores.value.map(({ value }) => value) }],
          areaStyle: {
            color: 'rgba(219, 211, 234, 0.5)'
          },
          itemStyle: {
            color: 'transparent'
          },
          lineStyle: {
            color: '#4B2396'
          },  
          emphasis: {
            disabled: true
          }
        }
      ]
    };
  }


const metricIndex = shallowRef(0);
</script>

<template>
  <v-example id="radar" title="Radar chart" desc="(with Pinia integration)">
    <v-chart :option="getRadarData(metricIndex)" autoresize />
  </v-example>
</template>