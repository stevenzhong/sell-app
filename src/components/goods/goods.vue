<template>
	<div class="goods">
		<div class="menu-wrapper" ref="menuWrapper">
		  <ul>
		  	<li v-for="item in goods" class="menu-item border-1px">
		  		<span class="text"><span v-show="item.type >0" class="icon" :class="classMap[item.type]"></span>{{item.name}}</span>
		  	</li>
		  </ul>
		</div>
		<div class="foods-wrapper" ref="foodsWrapper">
			<ul>
				<li v-for="item in goods" class="food-list">
					<h1 class="title">{{item.name}}</h1>
					<ul>
						<li v-for="food in item.foods" class="food-item border-1px">
							<div class="icon">
								<img width="57" height="57" :src="food.icon">
							</div>
							<div class="content">
								<h2 class="name">{{food.name}}</h2>
								<p class="desc">{{food.description}}</p>
								<div class="extra">
									<span class="count">月售{{food.sellCount}}份</span>
									<span>好评率{{food.rating}}%</span>
								</div>
								<div class="price">
									<span class="now">￥{{food.price}}</span>
									<span class="old" v-show="food.oldPrice">￥{{food.oldPrice}}</span>
								</div>
							</div>
						</li>
					</ul>
				</li>
			</ul>
		</div>
	</div>
  
</template>

<script>
import BScroll from "better-scroll"
const ERR_OK =0;
export default {
	props:{
		seller:{
			type:Object
		}
	},
	data() {
		return {
			goods:[]
		}
	},
	created() {
		this.classMap = ['decrease', 'discount', 'special', 'invoice', 'guarantee']; 
		this.$http.get('/api/goods').then((response) => {
			response = response.body;
			if(response.errno === ERR_OK){
				this.goods = response.data;
				// DOM 更新了 操作dom时一定要在$nextTick里
                this.$nextTick(() => {
                    this._initScroll();
                });
			}
		})
	},
	methods: {
		_initScroll() {
			this.menuScroll = new BScroll(this.$refs.menuWrapper, {});
			this.foodsScroll = new BScroll(this.$refs.foodsWrapper, {});
		}
	}
}
</script>

<style rel="stylesheet/scss" lang="scss" scoped>

@import "../../common/scss/mixins";
	.goods{
		display:flex;
		position: absolute;
		top:174px;
		bottom:46px;
		overflow:hidden;
		width:100%;
		.menu-wrapper{
			flex:0 0 80px;
			width:80px;
			background:#f3f5f7;
			.menu-item{
				&.border-1px{
					@include border-1px(#ccc);
				}
				padding:0 12px;
				display:table;
				width:56px;
				height:54px;
				line-height:14px;
				.icon{
					display: inline-block;
                    vertical-align: top;
                    width: 12px;
                    height: 12px;
                    margin-right: 4px;
                    background-size: 12px 12px;
                    background-repeat: no-repeat;
                    &.decrease {
                        @include bg-image('decrease_3');
                    }
                    &.discount {
                        @include bg-image('discount_3');
                    }
                    &.guarantee {
                        @include bg-image('guarantee_3');
                    }
                    &.invoice {
                        @include bg-image('invoice_3');
                    }
                    &.special {
                        @include bg-image('special_3');
                    }
				}
				.text{
					font-size:12px;
					display:table-cell;
					width:56px;
					vertical-align:middle;
				}
			}
		}
		.foods-wrapper{
			flex:1;
			.title{
				padding-left:14px;
				height:26px;
				line-height:26px;
				border-left:2px solid #d9dde1;
				font-size:12px;
				color:rgb(147,153,159);
				background:#f3f5f7;
			}
			.food-item{
				display:flex;
				margin:18px;
				padding-bottom:18px;
				&.border-1px{
					@include border-1px(rgba(7,17,27,0.1));
				}
				&:last-child{
					@include border-none();
					margin-bottom:0;
				}
				.icon{
					flex:0 0 57px;
					margin-right:10px;
				}
				.content{
					flex:1;
					.name{
						margin:2px 0 8px 0;
						height:14px;
						line-height:14px;
						font-size:14px;
						color:rgb(7,17,27);
					}
					.desc{
						margin-bottom:8px;
						line-height:10px;
						font-size:10px;
						color:rgb(147,153,159);
					}
					.extra{
						line-height:10px;
						font-size:10px;
						color:rgb(147,153,159);
						.count{
							margin-right:12px;
						}
					}
					.price{
						font-weight:70px;
						line-height:24px;
						.now{
							margin-right:8px;
							font-size:14px;
							color:rgb(240,20,20);
						}
						.old{
							text-decoration:line-through;
							font-size:10px;
						}
					}
				}
			}
		}
	}
</style>