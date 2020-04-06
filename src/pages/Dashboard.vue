<template>
  <div id="pageDashboard">
    <v-container grid-list-xl fluid>
      <v-layout row wrap>
        <!-- mini statistic start -->
        <v-flex lg3 sm6 xs12>
          <mini-statistic icon="mdi-information-outline" :title="dashboard.views.page0" sub-title="Views" color="purple"> </mini-statistic>
        </v-flex>
        <v-flex lg3 sm6 xs12>
          <mini-statistic icon="mdi-github" :title="dashboard.views.page1" sub-title="Views" color="indigo"> </mini-statistic>
        </v-flex>
        <v-flex lg3 sm6 xs12>
          <mini-statistic icon="mdi-food" :title="dashboard.views.page2" sub-title="Views" color="red"> </mini-statistic>
        </v-flex>
        <v-flex lg3 sm6 xs12>
          <mini-statistic icon="mdi-monitor-dashboard" :title="dashboard.views.page3" sub-title="Views" color="light-blue"> </mini-statistic>
        </v-flex>
        <!-- mini statistic  end -->
        <v-flex lg4 sm12 xs12>
          <v-widget title="Curtidas em receitas por categoria" content-bg="white" >
            <div slot="widget-content">
              <e-chart v-if="dashboard.likes.meal.length > 0"
                :path-option="[
                  ['dataset.source', dashboard.likes.meal],
                  ['legend.bottom', '0'],
                  [
                    'color',
                    [
                      color.lightBlue.base,
                      color.indigo.base,
                      color.pink.base,
                      color.green.base,
                      color.cyan.base,
                      color.teal.base
                    ]
                  ],
                  ['xAxis.show', false],
                  ['yAxis.show', false],
                  ['series[0].type', 'pie'],
                  ['series[0].avoidLabelOverlap', true],
                  ['series[0].radius', ['50%', '70%']]
                ]"
                height="400px"
                width="100%"
              >
              </e-chart>
              <v-container v-else>
                <v-row no-gutters dense class="d-flex flex-column justify-center">
                  <v-col>
                    <h3 align="center">Sem dados para exibir</h3>
                  </v-col>
                  <v-col>
                    <p  class="text-md-center mt-3">Dê like em alguma receita na página Randow Meal Generator</p>
                    </v-col>
                    <v-icon size="60px" :color="save ? 'red' : '' ">mdi-heart</v-icon>
                </v-row>
                
              </v-container>
              
            </div>
          </v-widget>
        </v-flex>
        <v-flex lg8 sm12 xs12>
          <v-widget title="Avaliações em receitas por categoria" content-bg="white">
            <v-btn icon slot="widget-header-action">
              <v-icon class="text--secondary">mdi-refresh</v-icon>
            </v-btn>
            <div slot="widget-content">
              <e-chart
                :path-option="[
                  ['dataset.source', dashboard.ratings.meal],
                  ['color', [color.lightBlue.base, color.green.lighten1]],
                  ['legend.show', true],
                  ['xAxis.axisLabel.show', true],
                  ['yAxis.axisLabel.show', true],
                  ['grid.left', '2%'],
                  ['grid.bottom', '5%'],
                  ['grid.right', '3%'],
                  ['series[0].type', 'bar'],
                  ['series[0].areaStyle', {}],
                  ['series[0].smooth', true],
                  ['series[1].smooth', true],
                  ['series[1].type', 'bar'],
                  ['series[1].areaStyle', {}]
                ]"
                height="400px"
                width="100%"
              >
              </e-chart>
            </div>
          </v-widget>

        </v-flex>
        <!-- box chart -->
        <v-layout row wrap class="pa-5">
        <v-flex sm12>
          <h4>Box Chart</h4>
        </v-flex>
        <v-flex lg4 sm6 xs12>
          
          <box-chart
            title="Page views"
            sub-title="10%"
            icon="trending_up"
            :data="dataset.monthVisit"
            :chart-color="[color.blue.darken1]"
            type="area"
          >
          </box-chart>
          
        </v-flex>
        <v-flex lg4 sm6 xs12>
          <box-chart
            title="Monthly Sales"
            sub-title="10%"
            icon="trending_up"
            card-color="pink"
            :data="dataset.monthVisit"
            :chart-color="[color.pink.lighten2]"
            type="bar"
          >
          </box-chart>
        </v-flex>
        <v-flex lg4 sm6 xs12>
          <box-chart
            card-color="indigo"
            title="Page views"
            sub-title="10%"
            icon="trending_up"
            :data="dataset.monthVisit"
            :chart-color="[color.shades.white]"
            type="line"
          >
          </box-chart>
        </v-flex>
      </v-layout>
        
        <!-- social/weather card start -->
        <v-flex lg4 sm12 xs12>
          <box-chart
            card-color="indigo"
            title="Trending"
            sub-title="10%"
            icon="mdi-trending-up"
            :data="siteTrafficData"
            :chart-color="[color.indigo.lighten1]"
            type="line"
          >
          </box-chart>
        </v-flex>
        <v-flex lg4 sm12 xs12>
          
          <box-chart
            card-color="indigo"
            title="Trending"
            sub-title="10%"
            icon="mdi-trending-up"
            :data="siteTrafficData"
            :chart-color="[color.indigo.lighten1]"
            type="line"
          >
          </box-chart>
          <box-chart
            class="mt-4"
            card-color="pink"
            title="Page views"
            sub-title="10%"
            icon="mdi-trending-up"
            :data="siteTrafficData"
            :chart-color="[color.pink.darken1, 'rgba(255,255,255,0.3)']"
            gradient
            type="area"
          >
          </box-chart>
        </v-flex>
        <!-- statistic section -->
        <v-flex lg4 sm12 xs12>
          <mini-chart
            title="Monthly Sales"
            sub-title="10%"
            icon="mdi-trending-up"
            :data="dataset.monthVisit"
            :chart-color="color.blue.base"
            type="bar"
          >
          </mini-chart>
          <linear-statistic title="Sales" sub-title="Sales increase" icon="mdi-trending-up" color="success" :value="15">
          </linear-statistic>
          <linear-statistic
            class="my-4"
            title="Orders"
            sub-title="Increase"
            icon="mdi-trending-up"
            color="pink"
            :value="30"
          >
          </linear-statistic>
          <linear-statistic
            class="my-4"
            title="Revenue"
            sub-title="Revenue increase"
            icon="mdi-trending-up"
            color="primary"
            :value="50"
          >
          </linear-statistic>
          <linear-statistic
            class="mt-4"
            title="Cost"
            sub-title="Cost reduce"
            icon="mdi-trending-down"
            color="orange"
            :value="25"
          >
          </linear-statistic>
        </v-flex>
        <!-- Circle statistic -->
        <v-flex lg4 sm12 xs12 v-for="(item, index) in trending" :key="'c-trending' + index">
          <circle-statistic
            :title="item.subheading"
            :sub-title="item.headline"
            :caption="item.caption"
            :icon="item.icon.label"
            :color="item.linear.color"
            :value="item.linear.value"
          >
          </circle-statistic>
        </v-flex>
        
      </v-layout>
    </v-container>
  </div>
</template>

<script>
import API from "@/api"
import EChart from "@/components/chart/echart"
import MiniStatistic from "@/components/widgets/statistic/MiniStatistic"
import VWidget from "@/components/VWidget"
import Material from "vuetify/es5/util/colors"
import BoxChart from "@/components/widgets/chart/BoxChart"
import CircleStatistic from "@/components/widgets/statistic/CircleStatistic"
import LinearStatistic from "@/components/widgets/statistic/LinearStatistic"
import MiniChart from "@/components/widgets/chart/MiniChart"
import { StackData, SinData, monthVisitData, campaignData } from "@/api/chart"

export default {
  components: {
    VWidget,
    MiniStatistic,
    EChart,
    BoxChart,
    CircleStatistic,
    LinearStatistic,
    MiniChart
  },
  props:{
    dashboard:{
      type: Object
    }
  },
  data: () => ({
    chart1: [],
    dataset: {
        sinData: SinData,
        monthVisit: monthVisitData,
        campaign: campaignData,
        stackData: StackData
      },
    color: Material,
    selectedTab: "tab-1",
    linearTrending: [
      {
        subheading: "Sales",
        headline: "2,55",
        caption: "increase",
        percent: 15,
        icon: {
          label: "mdi-trending-up",
          color: "success"
        },
        linear: {
          value: 15,
          color: "success"
        }
      },
      {
        subheading: "Revenue",
        headline: "6,553",
        caption: "increase",
        percent: 10,
        icon: {
          label: "medi-trending-down",
          color: "error"
        },
        linear: {
          value: 15,
          color: "error"
        }
      },
      {
        subheading: "Orders",
        headline: "5,00",
        caption: "increase",
        percent: 50,
        icon: {
          label: "mdi-arrow-up",
          color: "info"
        },
        linear: {
          value: 50,
          color: "info"
        }
      }
    ],
    trending: [
      {
        subheading: "Email",
        headline: "15+",
        caption: "email opens",
        percent: 15,
        icon: {
          label: "mdi-email",
          color: "info"
        },
        linear: {
          value: 15,
          color: "info"
        }
      },
      {
        subheading: "Tasks",
        headline: "90%",
        caption: "tasks completed.",
        percent: 90,
        icon: {
          label: "mdi-format-list-bulleted",
          color: "primary"
        },
        linear: {
          value: 90,
          color: "success"
        }
      },
      {
        subheading: "Issues",
        headline: "100%",
        caption: "issues fixed.",
        percent: 100,
        icon: {
          label: "mdi-bug",
          color: "primary"
        },
        linear: {
          value: 100,
          color: "error"
        }
      }
    ]
  }),
  computed: {
    activity() {
      return API.getActivity()
    },
    posts() {
      return API.getPost(3)
    },
    siteTrafficData() {
      return API.getMonthVisit
    }
  }
}
</script>
