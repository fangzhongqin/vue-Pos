<template>
  <div class='pos'>
    <div>
      <el-row>
      <!--左侧的点餐栏-->
        <el-col :span='7' class="left" id="pos-left">
          <el-tabs>
            <el-tab-pane label="点餐">
              <el-table :data="tableData" border show-summary style="width:100%">
                <el-table-column prop="goodsName" label="商品" ></el-table-column>
                <el-table-column prop="price" label="单价" width="65"></el-table-column>
                <el-table-column prop="count" label="数量" width="65"></el-table-column>
                <el-table-column label="总价" width="65">
                  <template scope="scope">
                    {{scope.row.count*scope.row.price}}
                  </template>
                </el-table-column>
                <el-table-column label="操作"  width="115" fixed="right" align="center">
                  <template scope="scope">
                    <el-button type="success" size="small" style="margin:0;padding:5px" @click="addGoodsList(scope.row)">增加</el-button>
                    <el-button type="danger" size="small" style="margin:0;padding:5px" @click="delateSingleGoods(scope.row)">删除</el-button>
                  </template>
                </el-table-column>
              </el-table>
              <div class="btn">
                <el-button type="warning" >挂单</el-button>
                <el-button type="danger" @click="delateAllGoods()">删除</el-button>
                <el-button type="success" >结账</el-button>
             </div>
            </el-tab-pane>
            <el-tab-pane label="挂单">挂单</el-tab-pane>
            <el-tab-pane label="外卖">外卖</el-tab-pane>
          </el-tabs>
        </el-col>

        <!--右侧的商品列表-->
        <el-col :span='17'>
          <!--常用商品列表-->
          <div class="often-goods">
              <div class="title">常用商品</div>
              <div class="often-goods-list">
                <ul>
                  <li class="goods" v-for="item in oftenGoods" @click="addGoodsList(item)">
                    <span>{{item.goodsName}}</span>
                    <span class="o-price">{{"¥"+item.price}}</span>
                  </li>
                </ul>
              </div>
          </div>

         <!--所有商品-->
          <div class="all-goods">
            <el-tabs>
              <el-tab-pane label="汉堡">
                <ul class='cookList'>
                  <li v-for="good0 in type0Goods" @click="addGoodsList(good0)">
                      <span class="foodImg"><img :src=good0.goodsImg width="100%"></span>
                      <span class="foodName">{{good0.goodsName}}</span>
                      <span class="foodPrice">{{"￥"+good0.price+"元"}}</span>
                  </li>
                </ul>
              </el-tab-pane>
              <el-tab-pane label="小食">
                <ul class='cookList'>
                  <li v-for="good1 in type1Goods" @click="addGoodsList(good1)">
                      <span class="foodImg"><img :src=good1.goodsImg width="100%"></span>
                      <span class="foodName">{{good1.goodsName}}</span>
                      <span class="foodPrice">{{"￥"+good1.price+"元"}}</span>
                  </li>
              </ul>
              </el-tab-pane>
              <el-tab-pane label="饮料">
                <ul class='cookList'>
                  <li v-for="good2 in type2Goods" @click="addGoodsList(good2)">
                      <span class="foodImg"><img :src=good2.goodsImg width="100%"></span>
                      <span class="foodName">{{good2.goodsName}}</span>
                      <span class="foodPrice">{{"￥"+good2.price+"元"}}</span>
                  </li>
              </ul>
              </el-tab-pane>
              <el-tab-pane label="套餐">
                <ul class='cookList'>
                  <li v-for="good3 in type3Goods" @click="addGoodsList(good3)">
                      <span class="foodImg"><img :src=good3.goodsImg width="100%"></span>
                      <span class="foodName">{{good3.goodsName}}</span>
                      <span class="foodPrice">{{"￥"+good3.price+"元"}}</span>
                  </li>
              </ul>
              </el-tab-pane>
            </el-tabs>
          </div>
        </el-col>
      </el-row>
    </div>
  </div>
</template>
<script type="text/javascript">
import axios from "axios"
  export default{
    name:'pos',
    data(){
      return {
          tableData:[],
          oftenGoods:[],
          type0Goods:[],
          type1Goods:[],
          type2Goods:[],
          type3Goods:[],
      }
    },
    /*获取后台数据*/
    created(){
      axios.get("http://jspang.com/DemoApi/oftenGoods.php")
      .then(response=>{
        console.log(response)
        this.oftenGoods=response.data;
      },error=>{
        console.log(error);
        alert("网络出现错误");
      }),
      axios.get("http://jspang.com/DemoApi/typeGoods.php")
      .then(response=>{
        this.type0Goods=response.data[0];
        this.type1Goods=response.data[1];
        this.type2Goods=response.data[2];
        this.type3Goods=response.data[3];
      },error=>{
        console.log(error);
        alert('网络错误');
      })
    },
    mounted:function(){
      var posLeft=document.getElementById('pos-left');
      var posHeight=document.body.offsetHeight;
      posLeft.style.height=posHeight+'px';
    },
    methods:{
      addGoodsList(goods){
        var isHave=false;
        for(let i=0;i<this.tableData.length;i++){
          if(this.tableData[i].goodsId==goods.goodsId){
            isHave=true;
          }
        }
        if(isHave){
          let arr=this.tableData.filter(o=>o.goodsId==goods.goodsId);
          arr[0].count++;
        }
        else{
          let newGoods={goodsId:goods.goodsId,goodsName:goods.goodsName,price:goods.price,count:1};
          this.tableData.push(newGoods);
        }
      },
      delateSingleGoods(goods){
        this.tableData=this.tableData.filter(o=>o.goodsId!=goods.goodsId);
      },
      delateAllGoods(){
        this.tableData=[];
      }
    }

  }
</script>
<style>
  .left{
    background-color:#F9FAFC;
    border-right:1px solid #D3DCE6;

  }
  .btn{
    margin:5px;
    float:right;
  }

  .title{
       height: 21px;
       border-bottom:1px solid #D3DCE6;
       background-color: #F9FAFC;
       padding:10px;
       text-align: left;
   }
   .often-goods-list ul li{
      list-style: none;
      float:left;
      border:1px solid #E5E9F2;
      padding:10px;
      margin:12px;
      background-color:#fff;
      width:19%;
      cursor:pointer;
   }
  .o-price{
      color:#58B7FF;
      margin:0 15px;
      font-weight: bold;
   }
  .all-goods{
      clear:both;
      //margin-top: 150px;
   }
   .cookList li{
       list-style: none;
       width:23%;
       border:1px solid #E5E9F2;
       height: auot;
       overflow: hidden;
       background-color:#fff;
       padding: 2px;
       float:left;
       margin: 2px;
       cursor:pointer;

   }
   .cookList li span{

        display: block;
        float:left;
   }
   .foodImg{
       width: 40%;
   }
   .foodName{
       font-size: 18px;
       padding-left: 10px;
       color:brown;

   }
   .foodPrice{
       font-size: 16px;
       font-weight: bold;
       padding-left: 10px;
       padding-top:10px;
   }
</style>
