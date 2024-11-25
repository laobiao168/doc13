<template>
	<view>
		<view style="background: #f7f7f7;">
			<view class="searchbox">
				<view class="inner98">
					<view class="inputbox">
						<uni-icons type="search" size="20" style="margin-top: 6px; margin-left: 10px;"></uni-icons>
						<input class="input" @input="inputkw"/>
					</view>
					<view class="search" @click="search">搜索</view>
				</view>
			</view>
		</view>
		<view class="ulist inner98">
			<view class="item" v-for="(item,index) in list" :key="index" @click="goUcenter(item)">
				<view class="left">
					<view class="uinfo" >
						<image :src="item.avatar" mode="aspectFill" class="img"></image>
						<view class="name">{{item.username}}</view>
					</view>
				</view>
				<view class="right" >
					帖子
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				keyword:'',
				list:[],
				page:1
			};
		},
		methods:{
			inputkw(e){
				this.keyword = e.detail.value;
			},
			search(){
				this.list = [];
				this.page=1;
				this.getlist();
			},
			getlist(){
				this.$request('user/myfans', {tab:this.tab, page:this.page, keyword:this.keyword, type:1, lotteryType:this.lotteryType, order:4}).then((res)=>{
					this.list = this.list.concat(res.data);
					this.page++;
				});
			},
			toggleFocus(item){
				this.$request('user/toggleFocus',{user_id:item.id}).then((res)=>{
					this.search();
				})
			},
			goUcenter(item){
				uni.navigateTo({
					url:'../../pages/ucenter/ucenter?id='+item.id
				})
			},
		},
		onLoad() {
			this.getlist()
		}
	}
</script>

<style lang="scss">
.ulist .item{border-bottom: 1px solid #f7f7f7; display: flex; justify-content: space-between; padding: 5px 0;}
.ulist .item .img{width: 42px; height: 42px; border-radius: 42px; overflow: hidden;}
.ulist .item .uinfo{display: flex;}
.ulist .item .uinfo .name{margin-top: 10px; color: #333; font-size: 1rem; margin-left: 10px;}
.ulist .item .right{background-color: #07c160; height: 22px; text-align: center; line-height: 22px; border-radius: 8px; color: #fff; font-size: .7rem; padding: 0px 10px; margin-top: 10px;}
</style>
