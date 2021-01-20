<template>
	<view>
		<view class="changeLine1"></view>
	    <view class="header">
			<text space="nbsp">得分：{{score}}</text>
	    </view>
		<view class="changeLine2"></view>
		<view class="changeLine1"></view>
		<view class="img"><image :src="rubbish[index].src" mode="aspectFit"></image></view>
        <view class="rubbishName">{{rubbish[index].name}}</view>
		<view class="changeLine1"></view>
		<view class="changeLine1"></view>
		<view class="changeLine1"></view>
		<view class="changeLine2">
			<view class="uCountTime">
				<text space="nbsp">还剩{{time}}秒</text>
			</view>
		</view>
		<view class="changeLine1"></view>
		<view class="changeLine1"></view>
		<view class="trashCans">
			<view class="trashCan"><image style="width: 187rpx;height: 187rpx;" :src="hazardousImgUrl" @click="hazardousFunction"></image></view>
			<view class="trashCan"><image style="width: 187rpx;height: 187rpx;" :src="residualImgUrl" @click="residualFunction"></image></view>
			<view class="trashCan"><image style="width: 187rpx;height: 187rpx;" :src="householdFoodImgUrl" @click="householdFoodFunction"></image></view>
			<view class="trashCan"><image style="width: 187rpx;height: 187rpx;" :src="recyclableImgUrl" @click="recyclableFunction"></image></view>
		</view>
	    <uni-popup ref="popup" type="center" maskClick="false">
			<view class="popupbox">
				<view class="popupbox1">
					<view style="margin: auto;">
						游戏结束<br>
						您的得分：{{score}}
					</view>
				</view>
				<view class="popupbox2">
						<view class="popupbox2_1" @click="popupbox2_1Fun">
							<view style="margin: auto;">返回首页</view>
						</view>
					    <view class="popupbox2_2">
							<view style="margin: auto;" @click="popupbox2_2Fun">再来一局</view>
						</view>
				</view>
			</view>
		</uni-popup>
	</view>
</template>

<script>
	import uniPopup from "../../components/uni-popup/uni-popup.vue";
	export default{
		components:{
			uniPopup
		},
		methods:{
			onShow:function(){
				console.log("游戏页开始显示");
				console.log("onShow",uni.getStorageSync('storageIndex'));
				if((uni.getStorageSync('storageIndex') != null) && (uni.getStorageSync('storageIndex')<80)){
					this.index = uni.getStorageSync('storageIndex') + 1;
				}
				else{
					this.index = 0;
				}
				this.reshow();
			},
			onReady:function(){
				console.log("游戏页初次渲染完成");
				this.timeCount();
			},
			onUnload:function(){
				console.log("游戏页已卸载");
				uni.setStorageSync('storageIndex',this.index);
				console.log("storageIndex",uni.getStorageSync('storageIndex'));
			},
			onHide:function(){
				console.log("游戏页已隐藏");
				uni.setStorageSync('storageIndex',this.index);
			},
			reshow(){
				this.residualImgUrl="../../static/residual.png";
				this.householdFoodImgUrl="../../static/householdFood.png";
				this.recyclableImgUrl="../../static/recyclable.png";
				this.hazardousImgUrl="../../static/hazardous.png";
				this.time = 30;
				this.score = 0;
				this.$refs.popup.close();
			},
			refrash(){
				this.residualImgUrl="../../static/residual.png";
				this.householdFoodImgUrl="../../static/householdFood.png";
				this.recyclableImgUrl="../../static/recyclable.png";
				this.hazardousImgUrl="../../static/hazardous.png";
				uni.setStorageSync('storageIndex',this.index);
				if(uni.getStorageSync('storageIndex')<80){
					this.index = uni.getStorageSync('storageIndex') + 1;
				}
				else{
					this.index = 0;
				}
				this.time = 30;
				this.score = 0;
				this.$refs.popup.close();
			},
			reset(){
				this.residualImgUrl="../../static/residual.png";
				this.householdFoodImgUrl="../../static/householdFood.png";
				this.recyclableImgUrl="../../static/recyclable.png";
				this.hazardousImgUrl="../../static/hazardous.png";
				if(this.index<80){
					this.index++;
				}
				else{
					this.index = 0;
				}
			},
			timeMinus(){
				if(this.time>0){
					this.time--;
				}
				else{
					this.$refs.popup.open();
				}
			},
			timeCount(){
				if(this.time>=0){
					setInterval(this.timeMinus,1000,);
				}
			},
			popupbox2_1Fun(){
				uni.setStorageSync('storageIndex',this.index);
				console.log('onUnload',uni.getStorageSync('storageIndex'));
				uni.reLaunch({
					url:"../index/index"
				})
			},
			popupbox2_2Fun(){
				this.refrash();
			},
			residualFunction(){
				var type = this.rubbish[this.index].type;
				if(type == 1){
					this.residualImgUrl="../../static/residualHook.png";
					this.score++;
				}
				else if(type == 2){
					this.residualImgUrl="../../static/residualCross.png";
					this.householdFoodImgUrl="../../static/householdFoodHook.png";
				}
				else if(type == 3){
					this.residualImgUrl="../../static/residualCross.png";
					this.recyclableImgUrl="../../static/recyclableHook.png";
				}
				else{
					this.residualImgUrl="../../static/residualCross.png";
					this.hazardousImgUrl="../../static/hazardousHook.png";
				}
				setTimeout(this.reset,700,);
			},
			householdFoodFunction(){
				var type = this.rubbish[this.index].type;
				if(type == 1){
					this.householdFoodImgUrl="../../static/householdFoodCross.png";
					this.residualImgUrl="../../static/residualHook.png";
				}
				else if(type == 2){
					this.householdFoodImgUrl="../../static/householdFoodHook.png";
					this.score++;
				}
				else if(type == 3){
					this.householdFoodImgUrl="../../static/householdFoodCross.png";
					this.recyclableImgUrl="../../static/recyclableHook.png";
				}
				else{
					this.householdFoodImgUrl="../../static/householdFoodCross.png";
					this.hazardousImgUrl="../../static/hazardousHook.png";
				}
				setTimeout(this.reset,700,);
			},
			recyclableFunction(){
				var type = this.rubbish[this.index].type;
				if(type == 1){
					this.recyclableImgUrl="../../static/recyclableCross.png";
					this.residualImgUrl="../../static/residualHook.png";
				}
				else if(type == 2){
					this.recyclableImgUrl="../../static/recyclableCross.png";
					this.householdFoodImgUrl="../../static/householdFoodHook.png";
				}
				else if(type == 3){
					this.recyclableImgUrl="../../static/recyclableHook.png";
					this.score++;
				}
				else{
					this.recyclableImgUrl="../../static/recyclableCross.png";
					this.hazardousImgUrl="../../static/hazardousHook.png";
				}
				setTimeout(this.reset,700,);
			},
			hazardousFunction(){
				var type = this.rubbish[this.index].type;
				if(type == 1){
					this.hazardousImgUrl="../../static/hazardousCross.png";
					this.residualImgUrl="../../static/residualHook.png";
				}
				else if(type == 2){
					this.hazardousImgUrl="../../static/hazardousCross.png";
					this.householdFoodImgUrl="../../static/householdFoodHook.png";
				}
				else if(type == 3){
					this.hazardousImgUrl="../../static/hazardousCross.png";
					this.recyclableImgUrl="../../static/recyclableHook.png";
				}
				else{
					this.hazardousImgUrl="../../static/hazardousHook.png";
					this.score++;
				}
				setTimeout(this.reset,700,);
			}
		},
		data(){
			return{
				score:0,
				time:30,
				index:0,
				hazardousImgUrl:"../../static/hazardous.png",
				imgUrl:"../../static/logo.png",
				residualImgUrl:"../../static/residual.png",
				householdFoodImgUrl:"../../static/householdFood.png",
				recyclableImgUrl:"../../static/recyclable.png",
				rubbish:[
					{
						src:"../../static/accumulator04.png",
						type:4,
						name:"铅蓄电池"
					},
					{
						src:"../../static/apple02.png",
						type:2,
						name:"苹果"
					},
					{
						src:"../../static/bananaPeel02.png",
						type:2,
						name:"香蕉皮"
					},
					{
						src:"../../static/bandage01.png",
						type:1,
						name:"创口贴"
					},
					{
						src:"../../static/basketball03.png",
						type:3,
						name:"篮球"
					},
					{
						src:"../../static/battery04.png",
						type:4,
						name:"干电池"
					},
					{
						src:"../../static/biscuit02.png",
						type:2,
						name:"饼干"
					},
					{
						src:"../../static/bread02.png",
						type:2,
						name:"面包"
					},
					{
						src:"../../static/broom01.png",
						type:1,
						name:"扫把"
					},
					{
						src:"../../static/cake02.png",
						type:2,
						name:"蛋糕"
					},
					{
						src:"../../static/cannedFood03.png",
						type:3,
						name:"罐头盒"
					},
			        {
			        	src:"../../static/cans03.png",
			        	type:3,
			        	name:"易拉罐"
			        },
					{
						src:"../../static/capsuleMedicine04.png",
						type:4,
						name:"胶囊"
					},
					{
						src:"../../static/carton03.png",
						type:3,
						name:"纸箱"
					},
					{
						src:"../../static/ceramicBowl01.png",
						type:1,
						name:"陶瓷碗"
					},
					{
						src:"../../static/chili02.png",
						type:2,
						name:"辣椒"
					},
					{
						src:"../../static/chocolate02.png",
						type:2,
						name:"巧克力"
					},
					{
						src:"../../static/chopsticks01.png",
						type:1,
						name:"筷子"
					},
					{
						src:"../../static/cigaretteButts01.png",
						type:1,
						name:"烟蒂"
					},
					{
						src:"../../static/clip03.png",
						type:3,
						name:"夹子"
					},
					{
						src:"../../static/colorPalette01.png",
						type:1,
						name:"颜料板"
					},
					{
						src:"../../static/corn02.png",
						type:2,
						name:"玉米"
					},
					{
						src:"../../static/cosmetic01.png",
						type:1,
						name:"化妆品"
					},
					{
						src:"../../static/dirtyClothes01.png",
						type:1,
						name:"干衣服"
					},
					{
						src:"../../static/doll03.png",
						type:3,
						name:"布偶"
					},
					{
						src:"../../static/eggplant02.png",
						type:2,
						name:"茄子"
					},
					{
						src:"../../static/eggshell02.png",
						type:2,
						name:"蛋壳"
					},
					{
						src:"../../static/fishBone02.png",
						type:2,
						name:"鱼骨"
					},
					{
						src:"../../static/fluorescentLamp04.png",
						type:4,
						name:"荧光灯"
					},
					{
						src:"../../static/garbageBag01.png",
						type:1,
						name:"垃圾袋"
					},
					{
						src:"../../static/gasTank03.png",
						type:3,
						name:"煤气罐"
					},
					{
						src:"../../static/glassBottle03.png",
						type:3,
						name:"玻璃瓶"
					},
					{
						src:"../../static/glassJug03.png",
						type:3,
						name:"玻璃杯"
					},
					{
						src:"../../static/hairSpray01.png",
						type:1,
						name:"发胶"
					},
					{
						src:"../../static/handbag03.png",
						type:3,
						name:"手提包"
					},
					{
						src:"../../static/hat03.png",
						type:3,
						name:"帽子"
					},
					{
						src:"../../static/insecticide04.png",
						type:4,
						name:"杀虫剂"
					},
					{
						src:"../../static/ketchup02.png",
						type:2,
						name:"番茄酱"
					},
					{
						src:"../../static/lighter01.png",
						type:1,
						name:"打火机"
					},
					{
						src:"../../static/lock03.png",
						type:3,
						name:"锁"
					},
					{
						src:"../../static/makeupBrush01.png",
						type:1,
						name:"化妆刷"
					},
					{
						src:"../../static/medicalCottonSwabs01.png",
						type:1,
						name:"医用棉签"
					},
					{
						src:"../../static/medicalGauze01.png",
						type:1,
						name:"医用纱布"
					},
					{
						src:"../../static/medicalGloves01.png",
						type:1,
						name:"橡胶手套"
					},
					{
						src:"../../static/milkCarton03.png",
						type:3,
						name:"牛奶盒"
					},
					{
						src:"../../static/mirror01.png",
						type:1,
						name:"镜子"
					},
					{
						src:"../../static/newspaper03.png",
						type:3,
						name:"报纸"
					},
					{
						src:"../../static/orange02.png",
						type:2,
						name:"橘子"
					},
					{
						src:"../../static/paint04.png",
						type:4,
						name:"油漆"
					},
					{
						src:"../../static/paintBucket04.png",
						type:4,
						name:"油漆桶"
					},
					{
						src:"../../static/paperBag03.png",
						type:3,
						name:"纸袋"
					},
					{
						src:"../../static/peachCore01.png",
						type:1,
						name:"果核"
					},
					{
						src:"../../static/peanutShells02.png",
						type:2,
						name:"花生壳"
					},
					{
						src:"../../static/pear02.png",
						type:2,
						name:"梨子"
					},
					{
						src:"../../static/peaskin02.png",
						type:2,
						name:"豌豆皮"
					},
					{
						src:"../../static/perfumeBottle03.png",
						type:3,
						name:"香水瓶"
					},
					{
						src:"../../static/pills04.png",
						type:4,
						name:"药片"
					},
					{
						src:"../../static/plasticBasket03.png",
						type:3,
						name:"塑料篮子"
					},
					{
						src:"../../static/plasticBottle03.png",
						type:3,
						name:"饮料瓶"
					},
					{
						src:"../../static/plasticComb03.png",
						type:3,
						name:"塑料梳子"
					},
					{
						src:"../../static/potato02.png",
						type:2,
						name:"土豆"
					},
					{
						src:"../../static/potteLeaves02.png",
						type:2,
						name:"盆栽树叶"
					},
					{
						src:"../../static/schoolbag03.png",
						type:3,
						name:"书包"
					},
					{
						src:"../../static/shallot02.png",
						type:2,
						name:"葱"
					},
					{
						src:"../../static/shell01.png",
						type:1,
						name:"贝壳"
					},
					{
						src:"../../static/sponge01.png",
						type:1,
						name:"海绵"
					},
					{
						src:"../../static/sportsShoes03.png",
						type:3,
						name:"运动鞋"
					},
					{
						src:"../../static/strawberry02.png",
						type:2,
						name:"草莓"
					},
					{
						src:"../../static/sugarCane02.png",
						type:2,
						name:"甘蔗"
					},
					{
						src:"../../static/tea02.png",
						type:2,
						name:"茶叶"
					},
					{
						src:"../../static/thermometer04.png",
						type:4,
						name:"体温计"
					},
					{
						src:"../../static/toletPaper01.png",
						type:1,
						name:"厕纸"
					},
					{
						src:"../../static/tomato02.png",
						type:2,
						name:"西红柿"
					},
					{
						src:"../../static/toothbrush01.png",
						type:1,
						name:"牙刷"
					},
					{
						src:"../../static/toothpasteSkin01.png",
						type:1,
						name:"牙膏皮"
					},
					{
						src:"../../static/toys03.png",
						type:3,
						name:"玩具"
					},
					{
						src:"../../static/trashCan03.png",
						type:3,
						name:"垃圾桶"
					},
					{
						src:"../../static/watercolorPen01.png",
						type:1,
						name:"水彩笔"
					},
					{
						src:"../../static/watermelonRind02.png",
						type:2,
						name:"西瓜皮"
					},
					{
						src:"../../static/wok03.png",
						type:3,
						name:"锅"
					},
					{
						src:"../../static/woodenComb03.png",
						type:3,
						name:"木梳子"
					}
				]
			}
		}
	}
</script>

<style>
	.rubbishName{
		text-align: center;
		font-size: 50rpx;
	}
	.popupbox2_1{
		display: flex;
		text-align: center;
		font-size: 45rpx;
		color: #ffffff;
		width: 275rpx;
		background-color: #1AAD19;
	}
	.popupbox2_2{  
		display: flex;
		text-align: center;
		font-size: 45rpx;
		color: #ffffff;
		width: 277rpx;
		background-color: #FF2958;
	}
	.popupbox2{
		display: flex;
		flex-direction: row;
		height: 82rpx;
	}
	.popupbox1{
		display: flex;
		text-align: center;
		font-size: 55rpx;
		height: 220rpx;
		background-color: ;
	}
	.popupbox{
		height: 300rpx;
		width: 550rpx;
		background-color: #ffffff;
	}
	.changeLine1{
		height: 26rpx;
	}
	.header{
		height: 60rpx;
		width: 100%;
		background-color: ;
		font-size: 50rpx;
		text-align: center;
	}
	.changeLine2{
		height: 107rpx;
	}
	.img{
		text-align: center;
	}
	.trashCans{
		display: flex;
		flex-direction: row;
	}
	.trashCan{
		height:187rpx;
		width: 25%;
		
	}
	.uCountTime{
		font-size: 40rpx;
		text-align: center;
	}
</style>
