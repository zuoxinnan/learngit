<!-- 医疗详情 -->
<template>
  <div class="container">
    <div class="top-tab-wrap">
      <div class="detail-back-btn-bg" @click="backClick()"></div>
     <!-- <div class="share-btn" @click="shareClick()"></div>-->
    </div>
    <mt-swipe class="swiper-wrap" :auto="4000" :show-indicators="false">
      <template v-for="item in imgs">
        <mt-swipe-item class="img-wrap">
          <img :src="item" alt="">
        </mt-swipe-item>
      </template>
    </mt-swipe>
    <div class="top-wrap">
      <div class="top-item">
        <h3 class="h3-title">{{medicalData.title}}</h3>
        <div class="price-wrap">
          <span class="top-2-item">佣金</span><span class="top-2-text">¥ {{medicalData.commission}}</span>
        </div>
        <div class="commission-wrap" v-for="item in medicalData.packages">
          <span class="top-2-item">{{item.label}}</span><span class="top-2-text-2">{{item.value}}</span>
        </div>
      </div>
    </div>

    <!-- content -->
    <div class="content-wrap">
      <div class="tab-wrap">
        <template v-for="(item, index) in tabLists">
          <div class="tab-item-wrap">
            <span class="tab-item" :class="tabIndex == index ? 'tab-item-active' : ''" @click="tabClick(index)">{{item}}</span>
          </div>
        </template> 
      </div>
      <div class="content-text" v-html="content"></div>
    </div>

    <!-- bottom -->
    <div class="bottom-wrap">
      <div class="bottom-item">
      	<a href="tel:18610078154">
	        <i class="item-icon item-icon-phone"></i>
	        <div class="item-text">咨询</div>
        </a>
      </div>
      <div class="bottom-item" @click="medicalOrder()">
        <i class="item-icon item-icon-chat"></i>
        <div class="item-text">预约</div>
      </div>
      <div class="bottom-item bottom-item-share" @click="shareClick">
        <i class="item-icon item-icon-share"></i>
        <span class="item-text item-text-share">分享</span>
      </div>
    </div>
  </div>
</template>

<script>
import Vue from 'vue';
import Medical from '../utils/highMedical.js';
import { Swipe, SwipeItem } from 'mint-ui';

Vue.component(Swipe.name, Swipe);
Vue.component(SwipeItem.name, SwipeItem);
export default {
  data () {
    return {
      tabIndex: 0,
      title:'',
      tabLists: ['套餐介绍', '详情介绍', '会诊流程'],
      medicalData: {},
      imgs: [],
      content: '',
      shareTipShow: true,
      projectAdvantage: []
    };
  },

  components: {},

  computed: {},

  mounted() {
    Medical.detail(this.$route.params.id).then((res) => {
    	//console.log(res.data.data)
    	
      this.medicalData = res.data.data;
      this.title=res.data.data.title
      if(res.data.data.hasOwnProperty('imgs') && res.data.data.imgs){
      	  this.imgs = res.data.data.imgs;
      }
      
      this.contentShowText();
    })
  },

  methods: {
    tabClick(index) {
      this.tabIndex = index;
      this.contentShowText();
    },
    shareTip() {
      this.shareTipShow = false;
    },
    backClick(){
    	location.href="md://backList"
    },
    shareClick(){
    	location.href="md://sharePage/"+this.title+'?'+this.OnlineLink+'/#/highMedical_detail_public_share/'+this.$route.params.id;
    },
    contentShowText() {
      if (this.tabIndex == 0) {
        this.content = this.medicalData.packageInfo;
      } else if (this.tabIndex == 1) {
        this.content = this.medicalData.detailInfo;        
      } else {
        this.content = this.medicalData.consultationInfo;        
      }
    },
    medicalOrder() {
      this.$router.push({path: '/medical_order?id=' + this.$route.params.id + '&name=' + this.medicalData.title});
    }
  }
}

</script>
<style scoped>
  .swiper-wrap {
    width: 100%;
    height: 4.22rem;
  }
  .img-wrap {
    width: 100%;
    height: 4.22rem;
  }
  .img-wrap img {
    width: 100%;
    height: 100%;
  }
  .top-wrap {
    padding-left: 0.3rem;
    background: white;
  }
  .top-item {
    padding: 0.34rem 0 0.39rem;
    border-bottom: 1px solid #D8D8D8;
  }
  .top-item:last-child {
    border: 0;
  }
  .h3-title {
    margin-bottom: 0.18rem;
    font-size: 0.32rem;
  }
  .price-wrap, .commission-wrap, .time-wrap {
    margin-bottom: 0.1rem;
  }
  .top-2-item {
    margin-right: 0.1rem;
    font-size: 0.26rem;
    color: #999999;
  }
  .top-2-text {
    font-size: 0.26rem;
    color: #FF7800;
  }
  .top-2-text-2 {
    font-size: 0.26rem;
    color: #151515;
  }

  /* 优势 */
  .advantage-wrap {
    margin-top: 0.29rem;
    overflow: hidden;
  }
  .advantage-item {
    padding: 0.03rem 0.18rem 0.04rem 0.18rem;
    margin-right: 0.12rem;
    margin-bottom: 0.15rem;
    float: left;
    font-size: 0.24rem;
    border: 1px solid #000000;
    -webkit-border-radius: 1rem;
    -moz-border-radius: 1rem;
    border-radius: 1rem;
  }
  .advantage-item:nth-child(1) {
    border-color: #F5D4B7;
    color: #F5D4B7;
  }
  .advantage-item:nth-child(2) {
    border-color: #B1FFAD;
    color: #B1FFAD;
  }
  .advantage-item:nth-child(3) {
    border-color: #B0E3F3;
    color: #B0E3F3;
  }
  .advantage-item:nth-child(4) {
    border-color: #FCADAD;
    color: #FCADAD;
  }
  .advantage-item:nth-child(5) {
    border-color: #DC8DDB;
    color: #DC8DDB;
  }
  .advantage-item:nth-child(6) {
    border-color: #EECB21;
    color: #EECB21;
  }

  .content-wrap {
    padding-bottom: 1.3rem;
    border-top:0.2rem solid #F0F0F0;
    background: white;
  }
  /* tab */
  .tab-wrap {
    display: flex;
    font-weight: 900;
    border-bottom: 1px solid #D8D8D8;
  }
  .tab-item-wrap {
    padding: 0.4rem 0 0.16rem;
    text-align: center;
    flex: 1;
  }
  .tab-item {
    padding: 0 6px;
    padding-bottom: 0.14rem;
  }
  .tab-item-active {
    color: #20AEE5;
    border-bottom: 3px solid #20AEE5;
  }
  .content-text {
    padding: 0.4rem 0.3rem;
    line-height: 0.53rem;
    box-sizing:border-box
  }
  .content-text img {max-width:100%;}

  /* bottom */
  .bottom-wrap {
    position: fixed;
    bottom: 0;
    width: 100%;
    height: 0.98rem;
    border-top: 1px solid #D8D8D8;
    background: white;
  }
  .share-tip {
    position: absolute;
    top: -0.78rem;
    left: 3.7rem;
    width: 3.17rem;
    height: 0.845rem;
    background: url("../assets/images/detail/share-tip.png") no-repeat center;
    background-size: cover;
  }
  .bottom-item {
    float: left;
    width: 30%;
    height: 0.98rem;
    text-align: center;
  }
  .bottom-item-share {
    margin-top: -0.02rem;
    width: 40%;
    height: 0.99rem;
    line-height: 0.99rem;
    background: #20AEE5;
  }
  .item-icon {
    display: block;
    margin: 0.14rem auto 0.01rem;
    width: 0.44rem;
    height: 0.44rem;
  }
  .item-icon-chat {
    background: url("../assets/images/detail/chat.png") no-repeat center;
    background-size: cover;
  }
  .item-icon-phone {
    background: url("../assets/images/detail/phone.png") no-repeat center;
    background-size: cover;
  }
  .item-icon-share {
    float: left;
    margin-top: 0.28rem;
    margin-left: 0.7rem;
    background: url("../assets/images/detail/share.png") no-repeat center;
    background-size: cover;
  }
  .item-text {
    color: #999999;
    font-size: 0.2rem;
  }
  .item-text-share {
    margin-left: -0.48rem;
    color: #FFFFFF;
    font-size: 0.3rem;
  }
</style>