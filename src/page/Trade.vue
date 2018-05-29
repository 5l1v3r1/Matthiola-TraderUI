<template>
  <div>
    <el-container>
      <el-header class="header">
        <Header></Header>
      </el-header>
      <el-container>
        <el-aside class="aside">
          <NavMenu></NavMenu>
        </el-aside>
        <el-container>
          <el-main v-bind:class="{'main-collapse': changeMainClass, 'main-notCollapse': true}">
            <template>
              <el-row>
                <el-col :span="24">
                  <el-breadcrumb separator-class="el-icon-arrow-right">
                    <el-breadcrumb-item :to="{ path: '/home' }">Home</el-breadcrumb-item>
                    <el-breadcrumb-item :to="{ path: '/trade' }">Trade History</el-breadcrumb-item>
                  </el-breadcrumb>
                </el-col>
              </el-row>
            </template>
            <div style="padding-top: 20px">
              <el-cascader :options="futuresCascader" v-model="selectedFutures"
                           size="small" placeholder="Select futures" :show-all-levels="false"
                           @change="handleChange" style="float: left; padding-left: 10px"></el-cascader>
              <el-select v-model="selectedScope" placeholder="Select scope" size="small" style="float: left; padding-left: 20px">
                <el-option v-for="item in scopeList" :key="item.value" :label="item.label" :value="item.value"></el-option>
              </el-select>
              <el-button type="primary" plain size="small" @click="doFilter"
                         style="width: 80px; float: left; margin-left: 40px"><i class="el-icon-search"></i>  Filter</el-button>
            </div>
            <div style="padding-top: 50px">
              <el-table :data="tradeHistory" style="width: 100%; text-align: left" stripe tooltip-effect="light"
                        highlight-current-row height="450px" size="small" v-loading="listLoading">
                <el-table-column label="Trade ID" prop="trade_id" fixed width="200" header-align="center" show-overflow-tooltip></el-table-column>
                <el-table-column label="Broker" prop="broker" width="100" header-align="center" show-overflow-tooltip></el-table-column>
                <el-table-column label="Futures" prop="futures_id" width="100" header-align="center"></el-table-column>
                <el-table-column label="Price" prop="price" width="100" header-align="center"></el-table-column>
                <el-table-column label="Quantity" prop="quantity" width="100" header-align="center"></el-table-column>
                <el-table-column label="Trade Time" prop="trade_time" width="100" header-align="center" show-overflow-tooltip></el-table-column>
                <el-table-column label="Initiator" header-align="center">
                  <el-table-column label="Trader" prop="initiator.trader" width="100" header-align="center"></el-table-column>
                  <el-table-column label="Firm" prop="initiator.firm" width="100" header-align="center"></el-table-column>
                  <el-table-column label="Side" prop="initiator.side" width="100" header-align="center"></el-table-column>
                </el-table-column>
                <el-table-column label="Completion" header-align="center">
                  <el-table-column label="Trader" prop="completion.trader" width="100" header-align="center"></el-table-column>
                  <el-table-column label="Firm" prop="completion.firm" width="100" header-align="center"></el-table-column>
                  <el-table-column label="Side" prop="completion.side" width="100" header-align="center"></el-table-column>
                </el-table-column>
              </el-table>
            </div>
          </el-main>
        </el-container>
      </el-container>
      <el-footer class="footer">
        <Footer></Footer>
      </el-footer>
    </el-container>
  </div>
</template>

<script>
import Header from '@/components/Header'
import NavMenu from '@/components/NavMenu'
import Footer from '@/components/Footer'
import { requestTradeList } from '@/common/api'

export default {
  name: 'Trade',
  components: {
    NavMenu, Footer, Header
  },
  created () {
    document.title = 'Trade History'
  },
  computed: {
    changeMainClass () {
      return !this.$store.state.isCollapse;
    }
  },
  data () {
    return {
      scopeList: [
        {value: 'all', label: 'ALL'},
        {value: 'self_only', label: 'SELF ONLY'}
      ],
      selectedScope: '',
      futuresCascader: [],
      selectedFutures: '',
      listLoading: false,
      tradeHistory: [
        {
          'trade_id': 'E271129A-D050-4D05-AFEA-5FB630CDF895',
          'broker': 'Morgan Stanley',
          'quantity': 50,
          'price': 30.12,
          'futures_id': 'GC_SEP18',
          'initiator': {
            'firm': 'A',
            'side': 'buy',
            'trader': 'John'
          },
          'completion': {
            'firm': 'B',
            'side': 'sell',
            'trader': 'Doe'
          },
          'trade_time': '2012-01-01T12:00:00Z'
        },
        {
          'trade_id': 'E271129A-D050-4D05-AFEA-5FB630CDF895',
          'broker': 'Morgan',
          'quantity': 50,
          'price': 30.12,
          'futures_id': 'GC_SEP18',
          'initiator': {
            'firm': 'A',
            'side': 'buy',
            'trader': 'John'
          },
          'completion': {
            'firm': 'B',
            'side': 'sell',
            'trader': 'Doe'
          },
          'trade_time': '2012-01-01T12:00:00Z'
        },
        {
          'trade_id': 'E271129A-D050-4D05-AFEA-5FB630CDF895',
          'broker': 'Morgan Stanley',
          'quantity': 50,
          'price': 30.12,
          'futures_id': 'GC_SEP18',
          'initiator': {
            'firm': '',
            'side': 'sell',
            'trader': ''
          },
          'completion': {
            'firm': '',
            'side': 'buy',
            'trader': ''
          },
          'trade_time': '2012-01-01T12:00:00Z'
        }
      ]
    }
  },
  mounted () {
    /*
    this.listLoading = true;
    let initParams = {
      'futuresID': '',
      'selfOnly': ''
    };
    requestTradeList(initParams).then((res) => {
      if (res.status === 200 && res.data.status === 200) {
        this.tradeHistory = res.data.data;
      }
      this.listLoading = false;
    });
    */
  },
  methods: {
    doFilter () {
      /*
      let filterParams = {
        'futuresID': this.selectedFutures,
        'selfOnly': this.selectedScope
      };
      this.listLoading = true;
      requestTradeList(filterParams).then((res) => {
        if (res.status === 200 && res.data.status === 200) {
          this.tradeHistory = res.data.data;
        }
        this.listLoading = false;
      });
      */
    }
  }
}
</script>

<style scoped>

</style>