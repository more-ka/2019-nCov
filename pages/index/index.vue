<template>
	<view class="summary" v-if="summary">
    <!-- 疫情形势 -->
    <view class="header">
      <view>疫情动态<view class="i"></view></view>
      <view>实时播报<view class="i"></view></view>
      <view>疫情辟谣<view class="i"></view></view>
    </view>
    <view class="updateTime">
      数据更新时间: {{formatTime(updateTime)}}
    </view>
   <view class="info">
      <view class="confirmedCount">
        <text class="number">{{summary.confirmedCount}}</text>
      <text class="title">确诊病例</text>
      </view>
      <view class="suspectedCount">
        <text class="number">{{summary.suspectedCount}}</text>
     <text class="title">疑似病例</text>
      </view>
      <view class="curedCount">
        <text class="number">{{summary.curedCount}}</text>
        <text class="title">治愈人数</text>
      </view>
      <view class="deadCount">
        <text class="number">{{summary.deadCount}}</text>
        <text class="title">死亡人数</text>
      </view>
    </view>
    <view class="introduce">
      <view class="generalRemark">
        数据来源: {{summary.generalRemark}}
      </view>
      <view class="infaceSoruce">
        传染源: {{summary.infectSource}}
      </view>
      <view class="virus">
        病毒: {{summary.virus}}
      </view>
      <view class="passWay">
        传播途径: {{summary.passWay}}
      </view>
      <view class="remark1">
        易感人群: {{summary.remark1}}
      </view> 
      <view class="remark2">
        潜伏期: {{summary.remark2}}
      </view>
    </view>

    <!-- 各个省详情 -->
  <view class="chinaArea">
      <view class="text"><text class="i"></text>疫情地图</view>
      <image :src="summary.dailyPic" mode="widthFix" class="map"></image>
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
    
    <!-- 疫情新闻 -->
   <view class="news">
      <view class="text"><text class="i"></text>实时播报</view>
      <view v-for="(item,index) in news" :key="index" class="item clearfix">
        <view class="newsTitle">{{item.title}}</view>
        <view class="pubDate">{{formatTime(item.pubDate)}}</view>
        <view class="summary">{{item.summary}}</view>
        <view class="infoSource">信息来源: {{item.infoSource}}</view>
      </view>
    </view>
    <!-- 疫情辟谣 -->
    <view class="rumors">
      <view class="text"><view class="i"></view>疫情辟谣</view>
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
				summary: {"infectSource": "野生动物，可能为中华菊头蝠", "passWay": "经呼吸道飞沫传播，亦可通过接触传播，存在粪-口传播可能性", "dailyPic": "https://img1.dxycdn.com/2020/0203/561/3394511511061134801-135.png", "summary": "", "countRemark": "", "confirmedCount": 17238, "suspectedCount": 21558, "curedCount": 477, "deadCount": 361, "seriousCount": 2296, "suspectedIncr": 2014, "confirmedIncr": 2748, "curedIncr": 43, "deadIncr": 57, "seriousIncr": 2296, "virus": "新型冠状病毒 2019-nCoV", "remark1": "易感人群：人群普遍易感。老年人及有基础疾病者感染后病情较重，儿童及婴幼儿也有发病", "remark2": "潜伏期：一般为 3～7 天，最长不超过 14 天，潜伏期内存在传染性", "remark3": "", "remark4": "", "remark5": "", "generalRemark": "疑似病例数来自国家卫健委数据，目前为全国数据，未分省市自治区等", "abroadRemark": "", "marquee": [{"id": 44, "marqueeLabel": "日报", "marqueeContent": " 七日内治愈人数首次超越死亡人数", "marqueeLink": "https://mama.dxy.com/japi/platform/200720055?index=20200202"}], "updateTime": 1580700000588},
        allArea: [],
        updateTime: 1580386046695,
        chinaArea: [],
        aboradArea: [],
        provinceArray: [],
        news: [],
        rumors: []
			}
		},
    computed:{
    },
		onLoad() {
      let that = this
      that.allArea = defAera
      that.news = defNews.results
      that.rumors = defRumors.results
      // 获取疫情信息
      // uni.request({
      //   url: "https://lab.isaaclin.cn/nCoV/api/overall",
      //   success(response) {
      //     that.summary = response.data.results[0]
      //     // console.log(that.summary);
      //     that.updateTime = that.summary.updateTime
      //   }
      // }),
      
      // 获取城市疫情
      // uni.request({
      //   url: "https://lab.isaaclin.cn/nCoV/api/area",
      //   success(response) {
      //     that.allArea = response.data.results
      //   },
      //   fail(err){
      //     that.allArea = defAera
      //   },
      //   complete() {
      //     that.formatCountryCount()
      //     that.formatCountry()
      //   }
      // })
      
      // 获取疫情新闻
      // uni.request({
      //   url: "https://lab.isaaclin.cn/nCoV/api/news",
      //   success(response) {
      //     that.news = response.data.results
      //     console.log(that.news);
      //   },
      //   fail(err){
      //     console.log(err);
      //   },
      //   complete() {
          
      //   }
      // })
      
      // 获取疫情辟谣
      uni.request({
        url: "https://lab.isaaclin.cn/nCoV/api/rumors",
        success(response) {
          that.rumors = response.data.results
          console.log(that.rumors,'---');
        },
        fail(err){
          console.log(err);
        },
        complete() {
          
        }
      })
      
          setTimeout(()=>{
            that.formatCountryCount()
            console.log(that.allArea);
            that.formatCountry()
          },3000)
		},
		methods: {
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
            // let allArea = this.chinaArea.length
            var arr = this.allArea
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
                    console.log(SelectionSort(arr))
      },
      formatCountry(){
        let allArea = this.allArea
        for(let item in allArea){
          if(allArea[item].country==="中国"){
            this.chinaArea.push(allArea[item])
          }else{
            this.aboradArea.push(allArea[item])
          }
        }
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
    onPageScroll(e) {
      console.log('滚动',e);
    }
	}
</script>

<style>
  @import url("index.css");
</style>
