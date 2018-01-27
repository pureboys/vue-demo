<template>
    <div class="pos">
        <div>
            <el-row>
                <el-col :span='7' class="pos-order" id="orderList">
                    <el-tabs type="card">
                        <el-tab-pane label="点餐">
                            <el-table :data="tableData" border show-summary :summary-method="getAllPrice"
                                      style="width: 100%">
                                <el-table-column prop="goodsName" label="商品名称" header-align="center"></el-table-column>
                                <el-table-column prop="count" label="数量" width="50"
                                                 header-align="center"></el-table-column>
                                <el-table-column prop="price" label="金额" width="70"
                                                 header-align="center"></el-table-column>
                                <el-table-column label="操作" width="100" header-align="center" fixed="right">
                                    <template slot-scope="scope">
                                        <el-button type="text" size="small" @click="delSingleGood(scope.row)">删除
                                        </el-button>
                                        <el-button type="text" size="small" @click="addOrderList(scope.row)">增加
                                        </el-button>
                                    </template>
                                </el-table-column>
                            </el-table>
                            <div class="div-btn">
                                <el-button type="warning">挂单</el-button>
                                <el-button type="danger" @click="delAllGoods">删除</el-button>
                                <el-button type="success" @click="checkout">结账</el-button>
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
                                <li v-for="goods in oftenGoods" @click="addOrderList(goods)">
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
                                        <li v-for="goods in type0Goods" @click="addOrderList(goods)">
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
                                        <li v-for="goods in type1Goods" @click="addOrderList(goods)">
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
                                        <li v-for="goods in type2Goods" @click="addOrderList(goods)">
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
                                        <li v-for="goods in type3Goods" @click="addOrderList(goods)">
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
                totalCount: 0,
                totalMoney: 0,
            }
        },
        created() {
            axios.get('http://localhost:3000/static/oftenGoods.json').then(response => {
                this.oftenGoods = response.data
            }).catch(error => {
                alert('网络错误~');
            });

            axios.get('http://localhost:3000/static/typeGoods.json').then(response => {
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
        },
        methods: {
            addOrderList(goods) {
                this.totalCount = 0;
                this.totalMoney = 0;
                let isHave = false;
                // 判断是否存在订单列表中
                for (let i = 0; i < this.tableData.length; i++) {
                    if (this.tableData[i].goodsId === goods.goodsId) {
                        isHave = true;
                    }
                }
                // 根据isHave的值判断订单列表中是否已经有此商品
                if (isHave) {
                    let arr = this.tableData.filter(o => o.goodsId === goods.goodsId); //存在就进行数量添加
                    arr[0].count++;
                } else {
                    let newGoods = {
                        goodsId: goods.goodsId,
                        goodsName: goods.goodsName,
                        price: goods.price,
                        count: 1
                    };
                    this.tableData.push(newGoods);
                }
            },
            getAllPrice(param) {
                //进行数量和价格的汇总计算
                const {columns, data} = param;
                const sums = []; // 最后一行
                columns.forEach((column, index) => {
                    if (index === 0) {
                        sums[index] = '总计';
                    } else if (index === 1) {
                        let counts = data.map(item => item.count);
                        //console.log(counts);
                        this.totalCount = sums[index] = counts.reduce((prev, curr) => {
                            return prev + curr;
                        }, 0);
                    } else if (index === 2) {
                        let values = data.map(item => item.count * item.price);
                        this.totalMoney = sums[index] = values.reduce((prev, curr) => {
                            return prev + curr;
                        }, 0);
                    }
                });
                //console.log(data.map(item => item.goodsId));
                return sums;
            },
            delSingleGood(goods) {
                this.tableData = this.tableData.filter(o => o.goodsId !== goods.goodsId);
            },
            delAllGoods() {
                this.tableData = [];
                this.totalMoney = 0;
                this.totalCount = 0;
            },
            checkout() {
                if (this.totalCount !== 0) {
                    this.tableData = [];
                    this.totalMoney = 0;
                    this.totalCount = 0;
                    this.$message({
                        message: '结账成功，感谢你又为店里出了一份力!',
                        type: 'success'
                    })
                } else {
                    this.$message.error('不能空结。老板了解你急切的心情！')
                }
            }
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
        font-size: 16px;
        padding-left: 10px;
        color: brown;

    }

    .foodPrice {
        font-size: 16px;
        padding-left: 10px;
        padding-top: 10px;
    }
</style>
