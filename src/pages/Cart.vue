<template>
    <div class="product">
            <!-- 售完後預購數量 -->
        <div class="proc_order">
            <p v-show="item.order">預購數量: {{ item.order }}</p>
        </div>
            <!-- 商品圖片 -->
        <div class="proc_img">
            <img alt="ProcImg" :src="item.imgSrc" />
        </div>
        
        <div class="proc_inf">
            <!-- 倒數計時：0秒則停售 -->
            <countDownTimer class="proc_time" @timeUp="chageState($event)" ></countDownTimer>
            <!-- 商品名稱 -->
            <div class="proc_title">
                <span>商品名稱：{{ item.title }}</span>
            </div>
            <!-- 商品剩餘數量 -->
            <div class="proc_quantity">
                <span>剩餘數量：{{ item.quantity }}</span>
            </div>
            <!-- 購買按鈕->跳出modal -->
            <div class="proc_button">
                <el-button id="shopButton" type="primary" @click="dialogVisible = true">{{ sellState }}</el-button>
            </div>
            <div class="modal">
                <el-dialog :visible.sync="dialogVisible">
                    <span slot="title">我要{{ sellState }}</span>

                    <el-input-number id="submitButton" v-model="num" :min="0" :max="maxQunantity" step-strictly>購買</el-input-number>

                    <span slot="footer" class="dialog-footer">
                        <el-button type="primary" @click="submit">確定</el-button>
                    </span>
                </el-dialog>
            </div>
        </div>
    </div>
</template>

<script src="https://unpkg.com/vue"></script>
<script>
import countDownTimer from '../components/countDownTimer'

export default {
    name: 'Cart',
    components: {
        countDownTimer,
    },
    data() {
        return {
            // items
            item: {
                title: '檸檬',   // 商品圖
                imgSrc: require('../assets/lemon.png'), // 商品圖位置
                quantity: 10,   // 剩餘數量
                order: 0,       // 預購數量
            },
            // dialog
            dialogVisible: false, // 控制modal
            num: 0, // 購買數量
            // 賣場狀態
            sellState: '購買', // 銷售狀態,購買-預購-停售
            maxQunantity: 10,
        }
    },
    methods: {
        // 確定購買
        submit() {
            if( this.sellState === "購買" ) {
                this.item.quantity -= this.num
                this.maxQunantity -= this.num
                console.log("購買：", this.num, "剩餘購買量:", this.maxQunantity)
                this.num = 0
                this.dialogVisible = false
            } else if ( this.sellState === "預購" ) {
                this.item.order += this.num
                this.maxQunantity -= this.num
                console.log("預購：", this.num, "剩餘預購量:", this.maxQunantity)
                this.num = 0
                this.dialogVisible = false
            } else {
                console.log("Error in submit")
            }
        },
        chageState(state) {
            this.sellState = state
        },
    },
    watch: {
        // 剩餘商品已賣完->預購
        'item.quantity': function(val) {
            if( val === 0 && this.sellState != "停售" ) {
                this.sellState = "預購"
                this.maxQunantity = 99
                console.log("現貨已售完!")
            }
        },
        'item.order': function(val) {
            if( val === 99 ) {
                this.sellState = "停售"
                console.log("預購已滿額!")
            }
        },
        sellState: function(val) {
            if( val === "停售" ) {
                document.getElementById("shopButton").disabled = true
                this.dialogVisible = false
            } else {
                document.getElementById("shopButton").disabled = false
            }
        },
    }
}
</script>

<style scoped>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
.product{
    position: relative;
    border-radius: 5px;
    border: 3px solid #375eca;
    padding: 25px;
    background-color: #7caad4;
    height: 450px;
    width: 600px;
    
 }
.proc_order{
    position: absolute;
    height: 10%;
    right: 10%;
    font-size: 14px;
}
.product .proc_img{
    position: absolute;
    height: 300px;
    width: 250px;
    top: 80px;
    left: 5%;
    
}
.proc_inf {
    position: absolute;
    height: 300px;
    width: 250px;
    top: 80px;
    right: 5%;
    display: flex;
    flex-direction: column;
    justify-content: space-between;
}

.proc_inf .proc_time, .proc_title, .proc_quantity {
    background-color: antiquewhite;
    height: 65px;
    text-align: center;
    font-size: 24px;
    line-height: 65px;
}

.proc_button{
    height: 65px;
    width: 50%;
    text-align: end;
    align-self: flex-end;
    font-size: 30px;
    line-height: 65px;
}

.modal {
  text-align: center;
}

img {
    max-height: 300px;
    max-width: 250px;
}
</style>