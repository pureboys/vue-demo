<template>
    <div class="pos">
        <div>
            <el-row>
                <el-col :span='7' class="pos-order" id="orderList">
                    我是订单栏
                </el-col>
                <!--商品展示-->
                <el-col :span="17">
                    我是产品栏
                </el-col>
            </el-row>
        </div>
    </div>
</template>

<script>
    export default {
        name: 'Pos',
        data() {
            return {}
        },
        mounted(){
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
</style>
