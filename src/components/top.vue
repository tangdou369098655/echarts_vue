/* eslint-disable camelcase */
<template>
  <div class="container">
    <!-- 头部1 -->
    <div class="item item_top">
      <img src="../assets/img/top.jpg" alt class="w_100">
      <div class="index_item_topfont">
        <h4>新型冠状病毒</h4>
        <h4>疫情实时动态 · 省市地图</h4>
      </div>
    </div>
    <!-- 头部2 -->
    <!-- 中间 -->
    <div class="font_statement">
      <div class="statement_title">
        全国疫情状况
      </div>
      <div class="update_time">
        截止{{ mydata1 }}
      </div>
    </div>
    <!-- 中间 -->
    <!-- 卡片 -->
    <div class="myrow">
      <div class="tag">
        <p>{{ confirmedCount }}</p>
        确诊
      </div>
      <div class="tag">
        <p>{{ suspectedCount }}</p>
        疑似
      </div>
      <div class="tag">
        <p>{{ deadCount }}</p>
        死亡
      </div>
      <div class="tag">
        <p>{{ curedCount }}</p>
        治愈
      </div>
    </div>
    <!-- 卡片 -->
  </div>
</template>

<script>
// import Logo from '~/components/Logo.vue'
import axios from 'axios'

export default {
  components: {
    // Logo
  },
  data () {
    return {
      mydata1: '111',
      confirmedCount: '',
      suspectedCount: '',
      curedCount: '',
      deadCount: ''
    }
  },
  created () {
    this.setDate()
    this.getData()
  },
  methods: {
    getData () {
      axios.get('https://lab.ahusmart.com/nCoV/api/overall').then((res) => {
        if (res.status === 200) {
          const _ = res.data.results[0]
          this.confirmedCount = _.confirmedCount
          this.suspectedCount = _.suspectedCount
          this.curedCount = _.curedCount
          this.deadCount = _.deadCount
        }
        // eslint-disable-next-line no-console
        console.log(res)
      })
    },
    setDate () {
      this.mydata1 = this.getweek()
      // eslint-disable-next-line
      console.log(this.mydata1)
    },
    getweek (dateString) {
      let da = ''
      if (dateString === undefined) {
        // 需要修改时间，改为昨天
        const aaa = new Date() - 86400000
        const now = new Date(aaa)
        let nowM = now.getMonth() + 1
        // eslint-disable-next-line camelcase
        nowM = nowM < 10 ? '0' + nowM : nowM
        let nowD = now.getDate()
        nowD = nowD < 10 ? '0' + nowD : nowD
        da = now.getFullYear() + '-' + nowM + '-' + nowD
        // eslint-disable-next-line no-console
        console.log('昨天系统时间是(偷偷减掉了一天嘿嘿):' + da)
      } else {
        da = dateString.toString()
        // 日期格式2015-12-30
      }
      // 下面备用
      const date1 = new Date(
        da.substring(0, 4),
        parseInt(da.substring(5, 7)) - 1,
        da.substring(8, 10)
      )
      // 当前日期
      const date2 = new Date(da.substring(0, 4), 0, 1)
      // 1月1号
      // 获取1月1号星期（以周一为第一天，0周一~6周日）
      let dateWeekNum = date2.getDay() - 1
      if (dateWeekNum < 0) {
        dateWeekNum = 6
      }
      if (dateWeekNum < 4) {
        // 前移日期
        date2.setDate(date2.getDate() - dateWeekNum)
      } else {
        // 后移日期
        date2.setDate(date2.getDate() + 7 - dateWeekNum)
      }
      const d = Math.round((date1.valueOf() - date2.valueOf()) / 86400000)
      if (d < 0) {
        const date3 = date1.getFullYear() - 1 + '-12-31'
        return this.getweek(date3)
      } else {
        // 得到年数周数
        const year = date1.getFullYear()
        const week = Math.ceil((d + 1) / 7)
        // eslint-disable-next-line no-console
        console.log(year + '年第' + week + '周')
        return da
      }
    }
  }
}
</script>

<style lang='less' scoped>
*{
 padding:0;
  margin: 0;
}
h4{
 
  text-align: left;
}
.container {
  width: 100%;
  min-width: 12rem;
  // max-width: 50rem;
  margin: 0 auto;
  .item_top {
    position: relative;
    color: white;
    .index_item_topfont {
      position: absolute;
      color: white;
      bottom: 10%;
      font-size: 2rem;
      left: 1rem;
    }
    @media screen and (max-width: 550px) {
      .index_item_topfont {
        left: 0.8rem;
        h4 {
          font-size: 1rem;
        }
      }
    }
    @media screen and (max-width: 300px) {
      .index_item_topfont {
        left: 0.5rem;
        h4 {
          font-size: 0.5rem;
        }
      }
    }
  }
  .font_statement{
    position: relative;
    text-align: left;
    margin: 15px 15px;
    padding: 0px 0px 5px 10px;
    border-left: .2rem solid #f60;
    border-bottom: 1px solid #efefef;
    .statement_title{
      font-size: 20px;
      font-weight: bold;
    }
    .update_time{
      font-size: 12px;
      color: #b6b6b6;
      font-weight: normal;
      vertical-align: middle;
    }
  }
  .myrow{
    display: flex;
    align-items: center;
    flex-wrap: wrap;
    justify-content: center;
    .tag{
      width: 17%;
      margin:0.2rem 0;
      text-align: center;
      border-radius: 4px;
      padding: 10px 10px;
      background-color: #f3f3f3;
      font-size: 15px;
      // margin-right: 5px;
      p{
      color: red;
      font-size: 20px;
      }
    }
  }
  .myrow .tag:not(:nth-child(4n)) {
 margin-right: 5px;
}
}
/* 公共样式1 */
.w_100 {
  width: 100%;
}
.font_h1 {
  font-size: 2rem;
}
.font_h2 {
  font-size: 1.5rem;
}
@media screen and (max-width: 550px) {
  .font_h1 {
    font-size: 1rem;
  }
  .font_h2 {
    font-size: 0.8rem;
  }
}
@media screen and (max-width: 300px) {
  .font_h1 {
    font-size: 0.5rem;
  }
  
    .container .myrow .tag{
      width: 30%;}
     .container   .myrow .tag:not(:nth-child(4n)) {
 margin-right: 0px;
}
  .container .myrow .tag:not(:nth-child(2n)) {
 margin-right: 0.4rem;}
  .font_h2 {
    font-size: 0.2rem;
  }
}
/* 公共样式2*/
</style>
