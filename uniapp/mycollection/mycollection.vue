<template>
	<view>
		<view style="background: #f7f7f7;">
			<view class="inner98">
				<view class="grtopnav">
					<view class="item" :class="{cur:lotteryType==0}" @click="changeLottery(0)">全部</view>
					<view class="item" :class="{cur:lotteryType==6}" @click="changeLottery(6)">十二生肖</view>
					<view class="item" :class="{cur:lotteryType==2}" @click="changeLottery(2)">新澳</view>
					<view class="item" :class="{cur:lotteryType==5}" @click="changeLottery(5)">老澳</view>
					<view class="item" :class="{cur:lotteryType==1}" @click="changeLottery(1)">港彩</view>
					<view class="item" :class="{cur:lotteryType==3}" @click="changeLottery(3)">台彩</view>
				</view>
				<view class="tab">
					<view class="item" :class="{cur:tab==1}" @click="changeTab(1)">高手论坛</view>
					<view class="item" :class="{cur:tab==2}" @click="changeTab(2)">发现</view>
					<view class="item" :class="{cur:tab==3}" @click="changeTab(3)">资料大全</view>
					<view class="item" :class="{cur:tab==4}" @click="changeTab(4)">图解小组</view>
					<view class="item" :class="{cur:tab==5}" @click="changeTab(5)">图库</view>
				</view>
			</view>
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
		
		<view class="alist" style="background-color: #f7f7f7;">
			<view class="item" v-show="tab<4" v-for="(item,index) in list" :key="index" @click="godetail(item)">
				<view class="inner98">
					<view class="user">
						<view class="avatarbox">
							<image :src="item.avatar" mode="aspectFill" class="avatar"></image>
						</view>
						<view>
							<view class="nickname">{{item.username}}</view>
							<uni-dateformat :date="item.addtime*1000" format="yyyy-MM-dd" class="riqi"></uni-dateformat>
						</view>
					</view>
					<view class="title" >
						<text class="qi" v-show="item.qi">{{item.qi}}</text>
						<image src="../../static/img/zd.png" mode="aspectFill" class="zhiding" v-if="item.weight>0"></image>
						<image src="../../static/img/jing.png" mode="aspectFill" class="jinghua" v-if="item.jing==1"></image>
						<text class="wz">{{item.title}}</text>
					</view>
					<view class="info" >{{item.descp}}</view>
					<view class="imagebox" v-if="item.images.length>0">
						<image :src="img" class="image" v-for="(img,k) in item.images" :key="k" mode="aspectFill"></image>
					</view>
					<view class="dzsctxt">
						<view class="iicc zan"><uni-icons type="hand-up" size="16"></uni-icons>{{item.zancount}}</view>
						<view class="iicc view"><uni-icons type="star" size="16"></uni-icons>{{item.collectcount}}</view>
						<view class="iicc view"><uni-icons type="eye" size="16"></uni-icons>{{item.clickCount}}</view>
						<view class="iicc btnam" v-if="item.lotteryType==1">
							<image src="../../static/img/btnxg1.png" mode="aspectFill" class="icon"></image>
							港彩
						</view>
						<view class="iicc btnam" v-else-if="item.lotteryType==2">
							<image src="../../static/img/btnam1.png" mode="aspectFill" class="icon"></image>
							澳彩
						</view>
						<view class="iicc btnam" v-else-if="item.lotteryType==3">
							<image src="../../static/img/btntw1.png" mode="aspectFill" class="icon"></image>
							台彩
						</view>
						<view class="iicc btnam" v-else-if="item.lotteryType==4">
							<image src="../../static/img/btnxjp1.png" mode="aspectFill" class="icon"></image>
							新彩
						</view>
					</view>
				</view>
			</view>
			
			
			
			<view class="item" v-show="tab==5" v-for="(item,index) in tulist" :key="index" @click="godetail(item)">
				<view class="inner98">
					<view class="title" >
						<text class="wz">{{item.pictureName}}</text>
					</view>
					<view class="imagebox" >
						<image :src="item.pic" class="image" mode="aspectFill"></image>
					</view>
				</view>
			</view>
			
		</view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				tab:1,
				lotteryType:1,
				page:1,
				list:[],
				tulist:[],
				keyword:'',
				userId:0,
				userInfo:{}
			};
		},
		methods:{
			changeLottery(v){
				this.page=1;
				this.lotteryType = v;
				this.list = [];
				this.getlist();
			},
			inputkw(e){
				this.keyword = e.detail.value;
			},
			changeTab(v){
				this.page=1;
				this.tab = v;
				this.list = [];
				this.getlist();
			},
			search(){
				this.list = [];
				this.page=1;
				this.getlist();
			},
			getlist(){
				if(this.tab==1){
					//高手论坛
					this.$request('user/mycollectlist', {tab:this.tab, page:this.page, keyword:this.keyword, type:1, lotteryType:this.lotteryType, order:4}).then((res)=>{
						console.log(res);
						if(res.data.length>0){
							this.list = this.list.concat(res.data);
						}
						this.page++;
					});
				}else if(this.tab==2){
					//发现
					this.$request('user/mycollectlist', {tab:this.tab, page:this.page, keyword:this.keyword, type:3, lotteryType:this.lotteryType, order:4}).then((res)=>{
						console.log(res);
						if(res.data.length>0){
							this.list = this.list.concat(res.data);
						}
						this.page++;
					});
				}else if(this.tab==3){
					//资料大全
					this.$request('user/mycollectlist', {tab:this.tab, page:this.page, keyword:this.keyword, lotteryType:this.lotteryType}).then((res)=>{
						if(res.data.length>0){
							this.list = this.list.concat(res.data);
						}
						this.page++;
					});
				}else if(this.tab==4){
					//图解
					this.$request('user/mycollectlist', {tab:this.tab, page:this.page, keyword:this.keyword, type:2, lotteryType:this.lotteryType, order:4}).then((res)=>{
						console.log(res);
						if(res.data.length>0){
							this.list = this.list.concat(res.data);
						}
						this.page++;
					});
				}else if(this.tab==5){
					//图库
					this.$request('user/mycollectlist', {page:this.page, keyword:this.keyword,lotteryType:this.lotteryType, tab:this.tab}).then((res)=>{
						console.log(res);
						if(res.data.length>0){
							this.tulist = this.list.concat(res.data);
						}
						this.page++;
					});
				}
			},
			godetail(item){
				let pageType = 0;
				if(this.tab==3){
					pageType = 1;
				}else{
					pageType = 2;
				}
				if(this.tab<5){
					uni.navigateTo({
						url:'../../pages/corpusdetail/corpusdetail?id='+item.id+'&pageType='+pageType
					})
				}else{
					uni.navigateTo({
						url:'../../pages/picture/picture?picId='+item.pictureTypeId+'&ltype='+this.lotteryType
					})
				}
			},
			getUserInfo(){
				this.$request('user/userInfo', {id:this.userId}).then((res)=>{
					this.userInfo = res.data;
				})
			}
		},
		onLoad(res) {
			this.userId = res.id;
			this.getlist()
			this.getUserInfo()
		},
		onReachBottom() {
			this.getlist()
		}
	}
</script>

<style lang="scss">
.top{background-color: #07c160;}
.tab{display: flex; justify-content: space-between;}
.tab .item{color: #999; font-weight: bold; line-height: 40px; padding: 5px 0; text-align: center;}
.tab .item.cur{color: #07c160;}


</style>
