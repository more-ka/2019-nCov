<template>
  <view class="page">
<!--    <view class="statusBar" ></view> -->
	<view class="summary" id="summary" v-if="summary">
    <!-- 疫情形势 -->
    <view class="header">
      <view @click="summaryClick" class="summaryTitle" ref="summaryTitle">疫情动态<view class="i"></view></view>
      <view @click="newsClick" class="newsTitle" ref="newsTitle">实时播报<view class="i"></view></view>
      <view @click="rumorsClick" class="rumorsTitle" ref="rumorsTitle">疫情辟谣<view class="i"></view></view>
    </view>
    <view class="updateTime" id="updateTime">
      数据更新时间: {{formatTime(updateTime)}}
    </view>
   <view class="info">
     <view class="currentConfirmedCount">
       <text><text class="incr">&nbsp;</text></text>
       <text class="number">{{summary.currentConfirmedCount}}</text>
     <text class="title">现存确诊</text>
     </view>
      <view class="confirmedCount">
        <text v-if="summary.confirmedIncr">较昨日<text class="incr">{{'+'+summary.confirmedIncr}}</text></text>
        <text class="number">{{summary.confirmedCount}}</text>
      <text class="title">确诊</text>
      </view>
      <view class="suspectedCount">
        <text v-if="summary.suspectedIncr">较昨日<text class="incr">{{'+'+summary.suspectedIncr}}</text></text>
        <text class="number">{{summary.suspectedCount}}</text>
     <text class="title">疑似</text>
      </view>
      <view class="seriousCount">
        <text v-if="summary.seriousIncr">较昨日<text class="incr">{{'+'+summary.seriousIncr}}</text></text>
        <text class="number">{{summary.seriousCount}}</text>
        <text class="title">重症</text>
      </view>
      <view class="curedCount">
        <text v-if="summary.curedIncr">较昨日<text class="incr">{{'+'+summary.curedIncr}}</text></text>
        <text class="number">{{summary.curedCount}}</text>
        <text class="title">治愈</text>
      </view>
      <view class="deadCount">
        <text v-if="summary.deadIncr">较昨日<text class="incr">{{'+'+summary.deadIncr}}</text></text>
        <text class="number">{{summary.deadCount}}</text>
        <text class="title">死亡人数</text>
      </view>
    </view>
    <view class="introduce">
      <view class="generalRemark">
        <view class="icon">
          <view class="i"></view>
        </view>
        数据来源: {{summary.generalRemark}}
      </view>
      <view class="infaceSoruce">
        <view class="icon">
          <view class="i"></view>
        </view>
        {{summary.note2}}
      </view>
      <view class="virus">
        <view class="icon">
          <view class="i"></view>
        </view>
        {{summary.note1}}
      </view>
      <view class="passWay">
        <view class="icon">
          <view class="i"></view>
        </view>
        {{summary.note3}}
      </view>
      <view class="remark1">
        <view class="icon">
          <view class="i"></view>
        </view>
        {{summary.remark1}}
      </view> 
      <view class="remark2">
        <view class="icon">
          <view class="i"></view>
        </view>
        {{summary.remark2}}
      </view>
    </view>

    <!-- 各个省详情 -->
  <view class="chinaArea">
      <!-- <view class="text"><text class="i"></text>疫情地图</view> -->
      
      <swiper class="swiper" v-if="summary.dailyPics"
      :indicator-dots="true" :autoplay="true" :interval="3000" :duration="1000" indicator-color="rgba(0, 0, 0, .2)">
        <swiper-item v-for="(image,index) in summary.dailyPics" :key="index" >
          <view class="swiper-item">
            <image :src="image" class="image" mode="aspectFit"></image>
          </view>
        </swiper-item>
      </swiper>
      <view class="title">
        <view class="provinceShortName">地区</view>
        <view class="confirmedCount">确诊</view>
        <view class="curedCount">治愈</view>
        <view class="deadCount">死亡</view>
      </view>
     <view class="province" v-for="(country,index) in chinaArea" :key="index">
        <view class="row" @click="provinceClick" :data-province="country.provinceShortName">
            <image src="../../static/right.png" class="rightIcon" v-if="provinceArray.indexOf(country.provinceShortName) === -1 && country.cities.length !== 0"></image>
            <image src="../../static/right.png" class="downIcon" v-if="provinceArray.indexOf(country.provinceShortName) !== -1 && country.cities.length !== 0"></image>
          <view class="provinceShortName">
          {{country.provinceShortName}}</view>
          <view class="confirmedCount">{{country.confirmedCount}}</view>
          <view class="curedCount">{{country.curedCount}}</view>
          <view class="deadCount">{{country.deadCount}}</view>
        </view>
        <view class="country" v-for="(city,index) in country.cities" :key="index"
        v-if="provinceArray.indexOf(country.provinceShortName) !== -1"
        >
          <view class="row">
            <view class="countryName">{{city.cityName}}</view>
            <view class="countryConfirmedCount">{{city.confirmedCount}}</view>
            <view class="countryCuredCount">{{city.curedCount}}</view>
            <view class="countryDeadCount">{{city.deadCount}}</view>
          </view>
        </view>
      </view> 
    </view>
  <view class="aboradArea">
      <view class="text"><text class="i"></text>境外数据</view>
      <view class="title">
          <view class="provinceShortName">地区</view>
          <view class="confirmedCount">确诊</view>
          <view class="curedCount">治愈</view>
          <view class="deadCount">死亡</view>
        </view>
      <view class="province" v-for="(country,index) in aboradArea" :key="index">
        <view class="row">
        <view class="provinceShortName">{{country.provinceShortName}}</view>
        <view class="confirmedCount">{{country.confirmedCount}}</view>
        <view class="curedCount">{{country.curedCount}}</view>
        <view class="deadCount">{{country.deadCount}}</view>
        </view>
      </view>  
    </view>
    </view>
    <!-- 疫情新闻 -->
   <view class="news" id="news">
      <view class="text"><text class="i"></text>实时播报</view>
      <view v-for="(item,index) in news" :key="index" class="item clearfix">
        <view class="newsTitle">{{item.title}}</view>
        <view class="pubDate">{{formatTime(item.pubDate)}}</view>
        <view class="itemSummary">{{item.summary}}</view>
        <view class="infoSource">信息来源: {{item.infoSource}}</view>
      </view>
    </view>
    <!-- 疫情辟谣 -->
    <view class="rumors" id="rumors">
      <view class="text" id="link"><view class="i"></view>疫情辟谣</view>
      <view v-for="(item,index) in rumors" :key="index" class="item clearfix">
        <view class="tag"><text class="index">{{index+1}}</text></view>
        <view class="newsTitle">{{item.title}}</view>
        <view class="mainSummary">{{item.mainSummary}}</view>
        <view class="body">{{item.body}}</view>
      </view>
    </view>
  </view>

</template>

<script>
  
  import defAera from '../../common/allArea.js'
  import defNews from '../../common/news.js'
  import defRumors from '../../common/rumors.js'
	export default {
		data() {
			return {
				summary: {"currentConfirmedCount": 8187, "confirmedCount": 81163, "suspectedCount": 155, "curedCount": 69734, "deadCount": 3242, "seriousCount": 2622, "currentConfirmedIncr": -899, "confirmedIncr": 47, "suspectedIncr": 12, "curedIncr": 935, "deadIncr": 11, "seriousIncr": -208, "generalRemark": "1. 3 月 12 日国家卫健委确诊补订遗漏 12 例确诊病例（非 12 日新增），暂无具体省份信息。 2. 浙江省 12 例外省治愈暂无具体省份信息。", "abroadRemark": "", "remark1": "易感人群：人群普遍易感。老年人及有基础疾病者感染后病情较重，儿童及婴幼儿也有发病", "remark2": "潜伏期：一般为 3～7 天，最长不超过 14 天，潜伏期内可能存在传染性，其中无症状病例传染性非常罕见", "remark3": "宿主：野生动物，可能为中华菊头蝠", "remark4": "", "remark5": "", "note1": "病毒：SARS-CoV-2，其导致疾病命名 COVID-19", "note2": "传染源：新冠肺炎的患者。无症状感染者也可能成为传染源。", "note3": "传播途径：经呼吸道飞沫、接触传播是主要的传播途径。气溶胶传播和消化道等传播途径尚待明确。", "updateTime": 1584508572292},
        allArea: [],
        updateTime: 1581473181000,
        chinaArea: [],
        aboradArea: [],
        provinceArray: [],
        news: [],
        rumors: []
			}
		},
    computed:{
    },
    onReady() {
      this.nodeInViewport('summaryTitle')
      let viewportHeight
      uni.getSystemInfo({
        success(res) {
          viewportHeight = res.screenHeight
        }
      })
        uni.createIntersectionObserver(this).relativeTo('.scroll-view',{top: -viewportHeight + 40}).observe('#summary', (res) => {
          if(res.intersectionRatio > 0){ this.nodeInViewport('summaryTitle') }
        })
        uni.createIntersectionObserver(this).relativeTo('.scroll-view',{bottom: -viewportHeight + 40}).observe('#news', (res) => {
          if(res.intersectionRatio > 0){ this.nodeInViewport('newsTitle') }
        })
        uni.createIntersectionObserver(this).relativeTo('.scroll-view',{bottom: -viewportHeight + 40}).observe('#rumors', (res) => {
          if(res.intersectionRatio > 0){ this.nodeInViewport('rumorsTitle') }
        })
    },
		onLoad() {
      let that = this
      uni.showToast({
        title: '请稍后',
        icon: 'loading',
        mask: true
      })
      
      // 获取疫情信息
      uni.request({
        url: "https://lab.isaaclin.cn/nCoV/api/overall",
        success(response) {
          that.summary = response.data.results[0]
          that.updateTime = that.summary.updateTime
        },
        fail(err) {
          
        }
      }),
      
      // 获取城市疫情
      uni.request({
        url: "https://lab.isaaclin.cn/nCoV/api/area",
        success(response) {
          that.allArea = response.data.results
        },
        fail(err){
          that.allArea = defAera
        },
        complete() {
            that.formatCountryCount()
            that.formatCountry()
        }
      })
      
      // 获取疫情新闻
      uni.request({
        url: "https://lab.isaaclin.cn/nCoV/api/news",
        success(response) {
          that.news = response.data.results
        },
        fail(err){
          that.news = defNews.results
        },
        complete() {
          
        }
      })
      
      // 获取疫情辟谣
      uni.request({
        url: "https://lab.isaaclin.cn/nCoV/api/rumors",
        success(response) {
          that.rumors = response.data.results
        },
        fail(err){
          that.rumors = defRumors.results
        },
        complete() {
          
        }
      })
		},
		methods: {
      // 传入被点击的tab,该节点被active
      nodeInViewport(element){
        let allElement = this.$refs[element].$el.parentElement.childNodes
        for(let i=0;i<allElement.length;i++){
          allElement[i].classList.remove('active')
        }
        this.$refs[element].$el.classList.add('active')
      },
      summaryClick(){
        // this.nodeInViewport('summaryTitle')
        uni.pageScrollTo({
          duration: 300,
          scrollTop: this.summaryTop
        })
      },
      newsClick(){
        uni.pageScrollTo({
          duration: 300,
          scrollTop: this.newsTop - 1
        })
        uni.pageScrollTo({
          duration: 300,
          scrollTop: this.newsTop + 1
        })
      },
      rumorsClick(){
        // this.nodeInViewport('rumorsTitle')
        uni.pageScrollTo({
          duration: 300,
          scrollTop: this.rumorsTop + 5
        })
      },
      provinceClick(e){
        let that = this
        let province = e.currentTarget.dataset.province
        if(that.provinceArray.indexOf(province) === -1){
          that.provinceArray.push(province)
        }else{
          that.provinceArray = that.provinceArray.filter((item) => {
            return item != province
          });
        }
      },
      formatCountryCount(){
            var array = this.allArea
            function SelectionSort(array){
                        if(array.length<=1){
                            return array;
                        }
                        var max;
                        for(var i=0;i<array.length;i++){
                            for(var k=i+1;k<array.length;k++){
                                if(array[k].confirmedCount > array[i].confirmedCount){
                                    max=array[k];
                                    array[k]=array[i];
                                    array[i]=max;
                                }
                            }
                        }
                        return array;
                    }
                    SelectionSort(array)
      },
      formatCountry(){
        let allArea = this.allArea
        for(let item in allArea){
          if(allArea[item].countryName==="中国" || allArea[item].country==="中国"){
            this.chinaArea.push(allArea[item])
          }else{
            this.aboradArea.push(allArea[item])
          }
        }
        setTimeout(()=>{
          this.getNodeHeight()
        },500)
      },
      getNodeHeight(){
        let that = this
        uni.createSelectorQuery().select("#summary").boundingClientRect((res)=>{
          this.summaryTop = res.top  -40
        }).exec()
        uni.createSelectorQuery().select("#news").boundingClientRect((res)=>{
          this.newsTop = res.top -40
        }).exec()
        uni.createSelectorQuery().select("#rumors").boundingClientRect((res)=>{
          this.rumorsTop = res.top -40
        }).exec()
        uni.hideToast()
      },
      // 时间处理函数
      formatTime(Time){
        Date.prototype.Format = function (fmt) {
            var o = {
                    "M+": this.getMonth() + 1, // 月份
                    "d+": this.getDate(), // 日
                    "h+": this.getHours(), // 小时
                    "m+": this.getMinutes(), // 分
                    "s+": this.getSeconds(), // 秒
                    "q+": Math.floor((this.getMonth() + 3) / 3), // 季度
                    "S": this.getMilliseconds() // 毫秒
            };
            if (/(y+)/.test(fmt))
                fmt = fmt.replace(RegExp.$1, (this.getFullYear() + ""));
            for (var k in o)
                if (new RegExp("(" + k + ")").test(fmt)) fmt = fmt.replace(RegExp.$1, (RegExp.$1.length == 1) ? (o[k]) : (("00" + o[k]).substr(("" + o[k]).length)));
            return fmt;
        }
        let date = new Date(Time).Format('yy-MM-dd hh:mm:ss'); //"2018-11-15 17:40:00"
        return date
      }
		},
    onPageScroll(){
      
    }
	}
</script>

<style>
  @import url("index.css");
</style>
