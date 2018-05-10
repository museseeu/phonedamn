<template>
  <div>
    <el-container>
      <el-main>
        <el-row :gutter="12" type="flex" justify="center">
          <el-col :sm="12" :lg="6" class="text-left">
            <h1 class="text-center">電信解約金估算</h1>
            <div class="fb-share-button "  data-href="https://phonefee.musecloak.com/" data-layout="button" data-size="small" data-mobile-iframe="true"><a target="_blank" href="https://www.facebook.com/sharer/sharer.php?u=https%3A%2F%2Fphonefee.musecloak.com%2F&amp;src=sdkpreparse" class="fb-xfbml-parse-ignore">分享</a></div>
            <p>以下為模擬計算，無法視為正確金額。</p>
            <div>
              <p>電信商: {{mobile}}</p>
            </div>
            <div>
              <p>門號起始日期</p>
              <el-input type="date" name="muse" v-model="startdate" placeholder="門號起始日期" ></el-input>
            </div>
            <div>
              <p>合約時間</p>
              <el-select v-model="contractTotal" placeholder="合約時間">
                <el-option
                  v-for="item in options.contractTotalDay"
                  :key="item.id"
                  :label="item.year"
                  :value="item.totalday">
                </el-option>
              </el-select>
            </div>
            <div>
              <p>欲解約日期</p>
              <el-input type="date" v-model="enddate" placeholder="欲解約日期"></el-input>
            </div>
            <div>
              <p>每月補貼款/月</p>
              <el-input type="number" v-model="mobileSubsidize" placeholder="请输入内容"></el-input>
              <p>設備補貼總額</p>
              <el-input type="number" v-model="deviceSubsidizeCost" placeholder="请输入内容"></el-input>
            </div>
            <el-card class="box-card showbox text-center">
            <div slot="header" class="clearfix">
              <span >估算解約金總費用</span>
            </div>
            <div class="text-left" v-if="useDateGap && remainDay && deviceSubsidizeCost && contractTotal">
              <p>已使用天數: {{useDateGap}} 天</p>
              <p>剩餘天數: {{remainDay}} 天，相當於 {{usingMonth}} 個月</p>
              <p>解約金額: <b>$ {{calculator()}}</b></p>
              <p>均攤24個月估算/月：每月多 <b>{{Math.round(calculator()/24)}}</b> 元</p>
            </div>
            <div v-else>
              <p>填寫完整資訊，即時估算。</p>
            </div>
            </el-card>
            <div class="social" style="text-align: center">
              <div class="line-it-button"
              data-lang="zh_Hant"
              data-type="share-e"
              data-url="https://phonefee.musecloak.com/" style="display: none;"></div>
            </div>
          </el-col>
        </el-row>
      </el-main>
    </el-container>
  </div>
</template>

<script>
export default {
  name: 'Phone',
  data () {
    return {
      startdate: '2016-11-11',
      enddate: '2018-05-10',
      mobile: 'CHT電信',
      mobileSubsidize: 100,
      deviceSubsidizeCost: 4000,
      contractTotal: '',
      options: {
        contractTotalDay: [{
          year: '12月',
          totalday: 365
        }, {
          year: '24月',
          totalday: 730
        }, {
          year: '30月',
          totalday: 912
        }, {
          year: '36月',
          totalday: 1095
        }]
      }
    }
  },
  methods: {
    calculator () {
      if (this.contractTotal > 0) {
        let subsidizePerday = (this.mobileSubsidize / 30) * this.useDateGap
        let subsidizeCost = subsidizePerday + Number(this.deviceSubsidizeCost)
        let cancellationFee = Math.round(subsidizeCost) * this.reminPercentage
        return Math.round(cancellationFee)
      } else {
        return '請選擇合約時間'
      }
    },
    isNegInt (n) {
      return typeof n === 'number' && n < 0 && Number.isInteger(n)
    }
  },
  computed: {
    useDateGap () {
      let start = new Date(this.startdate)
      let end = new Date(this.enddate)
      return (end - start) / 86400000
    },
    remainDay () {
      return this.contractTotal - this.useDateGap
    },
    reminPercentage () {
      return (this.remainDay / this.contractTotal).toFixed(2)
    },
    usingMonth () {
      return (this.remainDay / 30).toFixed(2)
    }
  }
}
</script>

<style lang="scss">
  .text-center{
    text-align: center;
  }
  .text-left{
    text-align: left;
  }

  .item {
    margin-bottom: 18px;
  }

  .clearfix:before,
  .clearfix:after {
    display: table;
    content: "";
  }
  .clearfix:after {
    clear: both
  }

  .box-card {
    width: 100%;
  }

  .showbox{
    margin: 20px auto;
  }

</style>
