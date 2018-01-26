<template>
    <div class="pos">
        <div>
            <el-row>
                <el-col :span='7' class="pos-order" id="orderList">
                    <el-tabs type="card">
                        <el-tab-pane label="点餐">
                            <el-table :data="tableData" border show-summary style="width: 100%">
                                <el-table-column prop="goodsName" label="商品名称" header-align="center"></el-table-column>
                                <el-table-column prop="count" label="数量" width="50"
                                                 header-align="center"></el-table-column>
                                <el-table-column prop="price" label="金额" width="70"
                                                 header-align="center"></el-table-column>
                                <el-table-column label="操作" width="100" header-align="center" fixed="right">
                                    <template slot-scope="scope">
                                        <el-button type="text" size="small">删除</el-button>
                                        <el-button type="text" size="small">增加</el-button>
                                    </template>
                                </el-table-column>
                            </el-table>
                            <div class="div-btn">
                                <el-button type="warning">挂单</el-button>
                                <el-button type="danger">删除</el-button>
                                <el-button type="success">结账</el-button>
                            </div>
                        </el-tab-pane>
                        <el-tab-pane label="挂单">
                            挂单
                        </el-tab-pane>
                        <el-tab-pane label="外卖">
                            外卖
                        </el-tab-pane>
                    </el-tabs>
                </el-col>
                <!--商品展示-->
                <el-col :span="17">
                    <div class="often-goods">
                        <div class="title">常用商品</div>
                        <div class="often-goods-list">
                            <ul>
                                <li v-for="goods in oftenGoods">
                                    <span>{{goods.goodsName}}</span>
                                    <span class="o-price">￥{{goods.price}}</span>
                                </li>
                            </ul>
                        </div>
                    </div>

                    <div class="goods-type">
                        <el-tabs type="card">
                            <el-tab-pane label="汉堡">
                                <div>
                                    <ul class="cookList">
                                        <li v-for="goods in type0Goods">
                                            <span class="foodImg">
                                                <img :src="goods.goodsImg" alt="" width="100%">
                                            </span>
                                            <span class="foodName">{{goods.goodsName}}</span>
                                            <span class="foodPrice">￥{{goods.price}}</span>
                                        </li>
                                    </ul>
                                </div>
                            </el-tab-pane>
                            <el-tab-pane label="小食">
                                <div>
                                    <ul class="cookList">
                                        <li v-for="goods in type1Goods">
                                            <span class="foodImg">
                                                <img :src="goods.goodsImg" alt="" width="100%">
                                            </span>
                                            <span class="foodName">{{goods.goodsName}}</span>
                                            <span class="foodPrice">￥{{goods.price}}</span>
                                        </li>
                                    </ul>
                                </div>
                            </el-tab-pane>
                            <el-tab-pane label="饮料">
                                <div>
                                    <ul class="cookList">
                                        <li v-for="goods in type2Goods">
                                            <span class="foodImg">
                                                <img :src="goods.goodsImg" alt="" width="100%">
                                            </span>
                                            <span class="foodName">{{goods.goodsName}}</span>
                                            <span class="foodPrice">￥{{goods.price}}</span>
                                        </li>
                                    </ul>
                                </div>
                            </el-tab-pane>
                            <el-tab-pane label="套餐">
                                <div>
                                    <ul class="cookList">
                                        <li v-for="goods in type3Goods">
                                            <span class="foodImg">
                                                <img :src="goods.goodsImg" alt="" width="100%">
                                            </span>
                                            <span class="foodName">{{goods.goodsName}}</span>
                                            <span class="foodPrice">￥{{goods.price}}</span>
                                        </li>
                                    </ul>
                                </div>
                            </el-tab-pane>
                        </el-tabs>
                    </div>

                </el-col>
            </el-row>
        </div>
    </div>
</template>

<script>
    import axios from 'axios';

    export default {
        name: 'Pos',
        data() {
            return {
                tableData: [],
                oftenGoods: [],
                type0Goods: [],
                type1Goods: [],
                type2Goods: [],
                type3Goods: [],
            }
        },
        created() {
            axios.get('/static/tableData.json').then(response => {
                this.tableData = response.data
            }).catch(error => {
                alert('网络错误~');
            });
            axios.get('/static/oftenGoods.json').then(response => {
                this.oftenGoods = response.data
            }).catch(error => {
                alert('网络错误~');
            });
            axios.get('/static/typeGoods.json').then(response => {
                this.type0Goods = response.data[0];
                this.type1Goods = response.data[1];
                this.type2Goods = response.data[2];
                this.type3Goods = response.data[3];
            }).catch(error => {
                alert('网络错误~');
            });
        },
        mounted() {
            let orderHeight = document.body.clientHeight;
            document.getElementById('orderList').style.height = `${orderHeight}px`;
        }
    }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
    .pos-order {
        background: #f9fafc;
        border-right: 1px solid #c0ccda;
    }

    .div-btn {
        margin: 20px 0 0 0;
    }

    .title {
        height: 20px;
        border-bottom: 1px solid #D3DCE6;
        background-color: #F9FAFC;
        padding: 10px;
    }

    .often-goods-list ul li {
        list-style: none;
        float: left;
        border: 1px solid #E5E9F2;
        padding: 10px;
        margin: 5px;
        background-color: #fff;
    }

    .o-price {
        color: #58B7FF;
    }

    .goods-type {
        clear: both;
        padding: 20px 0 0 0;
    }

    .cookList li {
        list-style: none;
        width: 23%;
        border: 1px solid #E5E9F2;
        height: auto;
        overflow: hidden;
        background-color: #fff;
        padding: 2px;
        float: left;
        margin: 2px;

    }

    .cookList li span {

        display: block;
        float: left;
    }

    .foodImg {
        width: 40%;
    }

    .foodName {
        font-size: 18px;
        padding-left: 10px;
        color: brown;

    }

    .foodPrice {
        font-size: 16px;
        padding-left: 10px;
        padding-top: 10px;
    }
</style>
