<template>
  <div class="dashboard-editor-container">
    <panel-group :data="PanelGroupData" />
    <el-row :gutter="32">
      <el-col :xs="24" :sm="24" :lg="16">
        <div class="chart-wrapper">
          <line-chart :chart-data="lineChartData" />
        </div>
      </el-col>
      <el-col :xs="24" :sm="24" :lg="8">
        <div class="chart-wrapper">
          <pie-chart :chart-data="pieChartData" />
        </div>
      </el-col>

    </el-row>

  </div>
</template>

<script>
import { panelGroupData, flowStat, serviceStat } from '@/api/dashboard'

import PanelGroup from './components/PanelGroup'
import LineChart from './components/LineChart'
import PieChart from './components/PieChart'

export default {
  name: 'DashboardAdmin',
  components: {
    PanelGroup,
    LineChart,
    PieChart
  },
  data() {
    return {
      PanelGroupData: {
        'serviceNum': 0,
        'todayRequestNum': 0,
        'currentQPS': 0,
        'appNum': 0
      },
      lineChartData: {
        'title': '今日流量统计',
        'today': [],
        'yesterday': []
      },
      pieChartData: {
        'title': '服务占比',
        'legend': [],
        'series': []
      }
    }
  },
  created() {
    this.fetchPanelGroupData()
    this.fetchFlowStat()
    this.fetchServiceStat()
  },
  methods: {
    fetchPanelGroupData(id) {
      panelGroupData({ }).then(Response => {
        this.PanelGroupData = Response.data
        this.PanelGroupData.currentQPS = Response.data.currentQPS
      }).catch(() => {

      })
    },
    fetchFlowStat(id) {
      flowStat({ }).then(Response => {
        this.lineChartData.today = Response.today
        this.lineChartData.yesterday = Response.yesterday
      }).catch(() => {

      })
    },
    fetchServiceStat(id) {
      serviceStat({ }).then(Response => {
        this.pieChartData.legend = Response.data.legend
        this.pieChartData.series = Response.data.data
      }).catch(() => {

      })
    },
    handleSetLineChartData(type) {
      // this.lineChartData = lineChartData[type]
    }
  }
}
</script>

<style lang="scss" scoped>
.dashboard-editor-container {
  padding: 32px;
  background-color: rgb(240, 242, 245);
  position: relative;

  .github-corner {
    position: absolute;
    top: 0px;
    border: 0;
    right: 0;
  }

  .chart-wrapper {
    background: #fff;
    padding: 16px 16px 0;
    margin-bottom: 32px;
  }
}

@media (max-width:1024px) {
  .chart-wrapper {
    padding: 8px;
  }
}
</style>
