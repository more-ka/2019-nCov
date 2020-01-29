<template>
	<view class="summary" v-if="summary">
    <view class="confirmedCount">
      确诊病例: {{summary.confirmedCount}}
    </view>
    <view class="suspectedCount">
      疑似病例: {{summary.suspectedCount}}
    </view>
    <view class="deadCount">
      治愈人数: {{summary.deadCount}}
    </view>
    <view class="curedCount">
      死亡人数: {{summary.curedCount}}
    </view>
    <view class="updateTime">
      数据更新时间: {{updateTime}}
    </view>
    <view class="generalRemark">
      数据更新时间: {{summary.generalRemark}}
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
    <image :src="summary.dailyPic" mode=""></image>
  </view>
  <!-- view. -->
</template>

<script>
	export default {
		data() {
			return {
				summary: [],
        allArea: [],
        updateTime: ""
			}
		},
		onLoad() {
      let that = this
      uni.request({
        url: "https://lab.isaaclin.cn/nCoV/api/overall",
        success(response) {
          that.summary = response.data.results[0]
          console.log(that.summary);
          that.formatTime(that.summary.updateTime)
        }
      })
		},
		methods: {
      formatTime(){
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
        let date = new Date(1580303400491).Format('yy-MM-dd hh:mm:ss'); //"2018-11-15 17:40:00"
        this.updateTime = date
      }
		}
	}
</script>

<style>
	.content {
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
	}

	.logo {
		height: 200rpx;
		width: 200rpx;
		margin-top: 200rpx;
		margin-left: auto;
		margin-right: auto;
		margin-bottom: 50rpx;
	}

	.text-area {
		display: flex;
		justify-content: center;
	}

	.title {
		font-size: 36rpx;
		color: #8f8f94;
	}
</style>
