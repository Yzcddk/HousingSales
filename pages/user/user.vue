<template>
	<view>
		<view class="header" v-bind:class="{'status':isH5Plus}">
			<view class="userinfo">
				<view class="face" @tap="toSetting"><image :src="userinfo.face"></image></view>
				<view class="info">
					<view class="username" 
					@tap="toLogin">{{userinfo.username}}</view>
					<view class="integral"
					@tap="toLogin">注册后可体验更多服务</view>
				</view>
			</view>
			<view class="setting"
			@tap="toSetting"><image src="../../static/user/setting.png"></image></view>
		</view>
		<view class="orders">
			<view class="box">
				<view class="label" v-for="(row,index) in orderTypeLise" :key="row.name" hover-class="hover"  @tap="toOrderType(index)">
					<view class="icon"><view class="badge" v-if="row.badge>0">{{row.badge}}</view><image :src="'../../static/user/'+row.icon"></image></view>
					{{row.name}}
				</view>
			</view>
		</view> 
		<view class="list" v-for="(list,list_i) in severList" :key="list_i">
			<view class="li" v-for="(li,li_i) in list" @tap="toPage(list_i,li_i)" v-bind:class="{'noborder':li_i==list.length-1}"  hover-class="hover" :key="li.name" >
				<view class="icon"><image :src="'../../static/user/sever/'+li.icon"></image></view>
				<view class="text">{{li.name}}</view>
				<image class="to" src="../../static/user/to.png"></image>
			</view>
		</view>
	</view>
</template>
<script>
	export default {
		data() {
			return {
				//#ifdef APP-PLUS
				 //isH5Plus:true,
				//#endif
				//#ifndef APP-PLUS
				//isH5Plus:false,
				//#endif
				userinfo:{},
				orderTypeLise:[
					//name-标题 icon-图标 badge-角标
					{name:'新闻公告',icon:'news.png',badge:1},
					{name:'我的预约',icon:'wait.png',badge:2},
					{name:'我的消息',icon:'msg.png',badge:6},
					{name:'联系人',icon:'contact.png',badge:9},
					{name:'浏览历史',icon:'history.png',badge:0}
				],
				severList:[
					[
						{name:'房屋管理',icon:'house.png'},
						{name:'我的贴子',icon:'text.png'},
						{name:'保障',icon:'protect.png'},
						{name:'地图',icon:'map.png'},
						{name:'会员',icon:'member.png'},
					],
					[
						{name:'我的收藏',icon:'point.png'},
						{name:'优惠',icon:'quan.png'},
						{name:'签到',icon:'momey.png'},
						{name:'任务',icon:'renw.png'},
					],
					[
						// {name:'积分明细',icon:'mingxi.png'},
						{name:'抽奖',icon:'choujiang.png'},
						// {name:'收货地址',icon:'addr.png'},
						{name:'银行卡',icon:'bank.png'},
						{name:'安全中心',icon:'security.png'},
						{name:'在线客服',icon:'kefu.png'}
					]
				],
			};
		},
		//下拉刷新，需要自己在page.json文件中配置开启页面下拉刷新 "enablePullDownRefresh": true
		onPullDownRefresh() {
		    setTimeout(function () {
		        uni.stopPullDownRefresh();
		    }, 1000);
		},
		onPageScroll(e){
			//兼容iOS端下拉时顶部漂移
			this.headerPosition = e.scrollTop>=0?"fixed":"absolute";
			this.headerTop = e.scrollTop>=0?null:0;
			this.statusTop = e.scrollTop>=0?null:-this.statusHeight+'px';
		},
		onLoad() {
			this.statusHeight = 0;
			// #ifdef APP-PLUS
			this.showHeader = false;
			this.statusHeight = plus.navigator.getStatusbarHeight();
			// #endif
			// 加载
			this.init();
		},toSetting(){
				uni.navigateTo({
					url:'../../user/setting/setting'
				})
			},
		onReady(){
			//此处，演示,每次页面初次渲染都把登录状态重置
			uni.setStorage({
				key: 'UserInfo',
				data: false,
				success: function () {
				},
				fail:function(e){
				}
			});
		},
		onShow(){
			uni.getStorage({
				key: 'UserInfo',
				success: (res)=>{
					if(!res.data){
						if(this.isfirst){
							//this.toLogin();
						}
						return ;
					}
					this.user = res.data;
				},
				fail:(e)=>{
					//this.toLogin(); 
				}
			});
		},
		methods: {

			init() {
				//用户信息
				this.userinfo={
					face:'../../static/demo/userpic/user.jpg',
					username:"VIP会员10240",
					// integral:"1435"
				}		
			},
			toSetting(){
				uni.navigateTo({
					url:'./setting'
				})
			},
			// 登录
			toLogin(){
				uni.showToast({title: '请登录',icon:"none"});
				uni.navigateTo({
					url:'login'
				})
				this.isfirst = false;
			},
			isLogin(){
				//实际应用中,用户登录状态应该用token等方法去维持登录状态.
				const value = uni.getStorageSync('UserInfo');
				if (value) {
					return true;
				}
				return false
			},
			//用户点击订单类型
			toOrderType(index){
				uni.showToast({title: this.orderTypeLise[index].name});
			},
			//用户点击列表项
			toPage(list_i,li_i){
				uni.showToast({title: this.severList[list_i][li_i].name});
			}
		}
	}
</script>

<style lang="scss">
page{background-color:#fff}
.header{
	&.status{padding-top:var(--status-bar-height);}
	background-color:#22c993;width:92%;height:30vw;padding:0 4%;display:flex;align-items:center;
	.userinfo{
		width:90%;display:flex;
		.face{flex-shrink:0;width:15vw;height:15vw;
			image{width:100%;height:100%;border-radius:100%}
		}
		.info{
			display:flex;flex-flow:wrap;padding-left:30upx;
			.username{width:100%;color:#fff;font-size:40upx}
			.integral{display:flex;align-items:center;padding:0 20upx;height:40upx;color:#fff;background-color:rgba(0,0,0,0.1);border-radius:20upx;font-size:24upx}
		}
	}
	.setting{
		flex-shrink:0;width:6vw;height:6vw;
		image{width:100%;height:100%}
	}
}
.hover{background-color:#eee}
.orders{
	background-color:#22c993;
	width:92%;height:11vw;
	padding:0 4%;
	margin-bottom:calc(11vw + 40upx);
	display:flex;
	align-items:flex-start;
	// border-radius:0 0 100% 100%;
	margin-top: -1upx;
	.box{
		width:98%;
		padding:0 1%;
		height:22vw;
		background-color:#fefefe;
		// border-radius:24upx;
		box-shadow:0 0 20upx rgba(0,0,0,0.15);
		margin-bottom:40upx;
		display:flex;align-items:
		center;
		justify-content:center;
		.label{
			display:flex;align-items:center;justify-content:center;flex-flow:wrap;width:100%;height:16vw;color:#666666;font-size:26upx;
			.icon{
				position:relative;width:7vw;height:7vw;margin:0 1vw;
				.badge{position:absolute;width:4vw;height:4vw;background-color:#ec6d2c;top:-1vw;right:-1vw;border-radius:100%;font-size:20upx;color:#fff;display:flex;align-items:center;justify-content:center;z-index: 10;}
				image{width:7vw;height:7vw;z-index: 9;}
			}
		}
	}
}
.list{
	width:100%;border-bottom:solid 26upx #f1f1f1;
	.li{
		width:92%;height:100upx;padding:0 4%;border-bottom:solid 1upx #e7e7e7;display:flex;align-items:center;
	&.noborder{border-bottom:0}
		.icon{
			flex-shrink:0;width:50upx;height:50upx;
			image{width:50upx;height:50upx}
		}
		.text{padding-left:20upx;width:100%;color:#666}
		.to{flex-shrink:0;width:40upx;height:40upx}
	}
}
</style>
