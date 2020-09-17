<template>
    <view>
        <view class="uni-padding-wrap">
		</view>
        <picker-view v-if="visible" :indicator-style="indicatorStyle" :value="value" @change="bindChange">
            <picker-view-column>
                <view class="time-item" v-for="(item,index) in years" :key="index">{{item}}年</view>
            </picker-view-column>
            <picker-view-column>
                <view class="time-item" v-for="(item,index) in months" :key="index">{{item}}月</view>
            </picker-view-column>
            <picker-view-column>
                <view class="time-item" v-for="(item,index) in days" :key="index">{{item}}日</view>
            </picker-view-column>
			<picker-view-column>
			    <view class="time-item" v-for="(item,index) in hours" :key="index">{{item}}时</view>
			</picker-view-column>
			<picker-view-column>
			    <view class="time-item" v-for="(item,index) in minutes" :key="index">{{item}}分</view>
			</picker-view-column>
        </picker-view>
		<view class="uni-title">
			{{year}}年{{month}}月{{day}}日{{hour}}时{{minute}}分
		</view>
    </view>
</template>

<script>
    export default {
        data () {
            const date = new Date();
            const years = [];
            const year = date.getFullYear();
            const months = [];
            const month = date.getMonth() + 1;
            const days = [];
            const day = date.getDate();
			const hours = [];
			const hour = date.getHours();
			const minutes = [];
			const minute = date.getMinutes();
            for (let i = 1990; i <= date.getFullYear(); i++) {
                years.push(i);
            }
            for (let i = 1; i <= 12; i++) {
				if (i < 10) {
					i = "0" + i;
				}
                months.push(i);
            }
            for (let i = 1; i <= 31; i++) {
				if (i < 10) {
					i = "0" + i;
				}
                days.push(i);
            }
			for (let i = 0; i <= 23; i++) {
				if (i < 10) {
					i = "0" + i;
				}
			    hours.push(i);
			}
			for (let i = 0; i <= 59; i++) {
				if (i < 10) {
					i = "0" + i;
				}
			    minutes.push(i);
			}
            return {
                years,
                year,
                months,
                month,
                days,
                day,
				hours,
				hour,
				minutes,
				minute,
                value: [9999, month - 1, day - 1, hour - 1, minute - 1],
				/**
				 * 解决动态设置indicator-style不生效的问题
				 */
                visible: true,
                indicatorStyle: `height: ${Math.round(uni.getSystemInfoSync().screenWidth/(750/this.height))}px;`
            }
        },
		props: {
			// 动态获取日期栏高度
			height: {type: String, default: "100"}
		},
        methods: {
            bindChange (e) {
                const val = e.detail.value;
                this.year = this.years[val[0]];
                this.month = this.months[val[1]];
                this.day = this.days[val[2]];
				this.hour = this.hours[val[3]];
				this.minute = this.minutes[val[4]];
				this.days = [];
				if (this.year % 4 == 0 && ( this.year % 100 !=0 || this.year % 400 == 0) && this.month == 2) {
					for (let i = 1; i <= 29; i++) {
						if (i < 10) {
							i = "0" + i;
						}
						this.days.push(i);
					}	
				}
				else if (this.month == 2) {
					for (let i = 1; i <= 28; i++) {
						if (i < 10) {
							i = "0" + i;
						}
						this.days.push(i);
					}	
				}
				else if (this.month == 1 || this.month == 3 || this.month == 5 || this.month == 7 || this.month == 8 || this.month == 10 || this.month == 12 ) {
					for (let i = 1; i <= 31; i++) {
						if (i < 10) {
							i = "0" + i;
						}
						this.days.push(i);
					}	
				}
				else {
					for (let i = 1; i <= 30; i++) {
						if (i < 10) {
							i = "0" + i;
						}
						this.days.push(i);
					}	
				}
            }
        }
    }
</script>

<style>
    picker-view {
        width: 100%;
        height: 300rpx;
		margin-left: 20rpx;
    }
    .item {
        line-height: 100rpx;
        text-align: center;
    }
	.uni-title {
		margin-left: 100rpx;
		margin-top: 20rpx;
	}
</style>