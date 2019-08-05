<template>
  <div class="pos">
   <el-row>
		 <el-col :span="7" class="pos-order" id='order-list'>
			 <el-tabs>
				 <el-tab-pane  label='点餐'>
					 <el-table :data="tableData" border>
						 <el-table-column prop="goodsName" label='商品名称'></el-table-column>
						 <el-table-column prop="count" label='数量' width="50"></el-table-column>
						 <el-table-column prop="price" label='价格' width="70"></el-table-column>
						 <el-table-column label='操作' width="100" fixed="right">
							 <template >
								 <el-button type="text" size="small">增加</el-button>
								 <el-button type="text" size="small">删除</el-button>
							 </template>
						 </el-table-column>
					 </el-table>
					 <div class="totalDiv">
						 <small>数量：0元</small> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<small>金额：0元</small>
					 </div>
					 <div class="div-btn">
						 <el-button type="warning">挂单</el-button>
						 <el-button type="danger">删除</el-button>
						 <el-button type="success">结账</el-button>
					 </div >
				 </el-tab-pane>
				 <el-tab-pane label='挂单'>
				 				2	 
				 </el-tab-pane><el-tab-pane label='外卖'>
					 3
				 </el-tab-pane>
			 </el-tabs>
		 </el-col>
		 <el-col :span="17">
		 <div class="often-goods">
			 <div class="title">常用商品</div>
			 <div class="often-goods-list">
				 <ul>
					 <li v-for="goods in oftenGoods">
						 <span>{{goods.goodsName}}</span>
						 <span class="o-price">{{goods.price}}￥</span>
					 </li>
				 </ul>
			 </div >
				  </div >
						
						
				 <div class="goods-type">
			 <el-tabs>
				 <el-tab-pane label="汉堡">
				 <div>
					 <ul class="cookList">
						 <li v-for="goods in type0Goods">
							 <span class="foodImg"><img :src="goods.goodsImg"/>
							 </span>
							 <span class="foodImg">{{goods.goodsName}}</span>
							 <span class="foodImg">{{goods.price}}元</span>
						 </li>
					 </ul>
				 </div>
				 </el-tab-pane>
				 <el-tab-pane label="小吃">
					 <div>
					 					 <ul class="cookList">
					 						 <li v-for="goods in type1Goods">
					 							 <span class="foodImg"><img :src="goods.goodsImg"/>
					 							 </span>
					 							 <span class="foodImg">{{goods.goodsName}}</span>
					 							 <span class="foodImg">{{goods.price}}元</span>
					 						 </li>
					 					 </ul>
					 </div>
				 </el-tab-pane>
				 <el-tab-pane label="饮料">
					 <div>
					 					 <ul class="cookList">
					 						 <li v-for="goods in type2Goods">
					 							 <span class="foodImg"><img :src="goods.goodsImg"/>
					 							 </span>
					 							 <span class="foodImg">{{goods.goodsName}}</span>
					 							 <span class="foodImg">{{goods.price}}元</span>
					 						 </li>
					 					 </ul>
					 </div>
				 </el-tab-pane>
				 <el-tab-pane label="套餐">
					 <div>
					 					 <ul class="cookList">
					 						 <li v-for="goods in type3Goods">
					 							 <span class="foodImg"><img :src="goods.goodsImg"/>
					 							 </span>
					 							 <span class="foodImg">{{goods.goodsName}}</span>
					 							 <span class="foodImg">{{goods.price}}元</span>
					 						 </li>
					 					 </ul>
					 </div>
				 </el-tab-pane>
			 </el-tabs>
			 
		 </div>
		 </el-col>
	 </el-row>
  </div>
</template>

<script>
	import axios from 'axios'
export default {
  name: 'pos',
	data(){
		return{
			tableData:[
			// 	{
			// 	goodsName:'可口可乐',
			// 	price:3,
			// 	count:1
			// },
			// {
			// 	goodsName:'香辣鸡腿堡',
			// 	price:18,
			// 	count:1
			// },{
			// 	goodsName:'爱心薯条',
			// 	price:3,
			// 	count:1
			// },{
			// 	goodsName:'甜筒',
			// 	price:2,
			// 	count:1
			// },
			],
			oftenGoods:[],
			type0Goods:[],
			type1Goods:[],
			type2Goods:[],
			type3Goods:[]
		}
	},
	created:function(){
		axios.get('http://jspang.com/DemoApi/oftenGoods.php')
		.then(reponse=>{
			// console.log(reponse);
			this.oftenGoods=reponse.data;
		})
		.catch(error=>{
			// console.log(error);
			alert("网络错误，不能访问")
			})
			
			axios.get('http://jspang.com/DemoApi/typeGoods.php')
			.then(reponse=>{
				// console.log(reponse);
				this.type0Goods=reponse.data[0];
				this.type1Goods=reponse.data[1];
				this.type2Goods=reponse.data[2];
				this.type3Goods=reponse.data[3];
			})
			.catch(error=>{
				// console.log(error);
				alert("网络错误，不能访问")
			})
			
			
	},
  mounted:function(){
		var orderHeiht=document.body.clientHeight;
		document.getElementById('order-list').style.height=orderHeiht+'px';
	},
	methods:{
		addOrderList(goods){
			//商品是否存在在订单列表
			let isHave=false
			for(let i=0; i<this.tableData.length;i++){
				if(this.tableData[i].goodsId==goods.goodsId){
					isHave=true
				}
			}
			// //根据判断的值编写业务逻辑
			// if(isHave){
			// 	let arr = this.tableData.filter(o=>o.goodsId == goods.goodsId);
			// 	arr[0].count++;
			// }
			// else{
			// 	let newsGoods={goodsId:goods.goodsId,goodsName:goods.goodsName,price:goods.price,count:1},
			// 	// this.tableData.push(newGoods);
			// }
			// 
			
		}
	}
}
</script>

<style scoped>
	.pos-order{
		background-color: #f9fafc;
		border-right: 1px solid #c0ccda;
	}
	.div-btn{
		margin-top: 10px;
	}
	.title{
		/* height: 10px; */
		border-bottom: 1px solid #d3dce6;
		background-color: #F9FAFC;
		padding: 10px;
		text-align:left ;
	}
	.often-goods-list ul li{
		list-style: none;
		float: left;
		border: 1px solid #f9fafc;
		padding: 10px;
		margin: 10px;
		background-color: #fff;
	}
	.o-price{
		color: #5887ff;
	}
	.goods-type{
		clear:both ;
	}
	.foodImg img{
		width: 50px;
		height: 50px;
	}
	.cookList li{
		list-style: none;
		float: left;
	}

</style>

