<template>
  <div class="trade-container">
    <h3 class="title">填写并核对订单信息</h3>
    <div class="content">
      <h5 class="receive">收件人信息</h5>
      <div class="address clearFix" v-for="addr in addrList" :key="addr.id">
        <span class="username" :class="{ selected: addr.isDefault == 1 }">{{
          addr.consignee
        }}</span>
        <!-- 切换默认地址--排他思想 -->
        <p @click="changeDefault(addr)">
          <span class="s1">{{ addr.fullAddress }}</span>
          <span class="s2">{{ addr.phoneNum }}</span>
          <span class="s3" v-show="addr.isDefault == 1">默认地址</span>
        </p>
      </div>

      <div class="line"></div>
      <h5 class="pay">支付方式</h5>
      <div class="address clearFix">
        <span class="username selected">在线支付</span>
        <span class="username" style="margin-left: 5px">货到付款</span>
      </div>
      <div class="line"></div>
      <h5 class="pay">送货清单</h5>
      <div class="way">
        <h5>配送方式</h5>
        <div class="info clearFix">
          <span class="s1">天天快递</span>
          <p>配送时间：预计8月10日（周三）09:00-15:00送达</p>
        </div>
      </div>
      <div class="detail">
        <h5>商品清单</h5>
        <ul
          class="list clearFix"
          v-for="order in detailOrderList"
          :key="order.skuId"
        >
          <li>
            <img
              :src="order.imgUrl"
              alt=""
              style="width: 100px; height: 100px"
            />
          </li>
          <li>
            <p>
              {{ order.skuName }}
            </p>
            <h4>7天无理由退货</h4>
          </li>
          <li>
            <h3>￥5399.00</h3>
          </li>
          <li>X{{ order.skuNum }}</li>
          <li>有货</li>
        </ul>
      </div>
      <div class="bbs">
        <h5>买家留言：</h5>
        <textarea
          placeholder="建议留言前先与商家沟通确认"
          class="remarks-cont"
          v-model="msg"
        ></textarea>
      </div>
      <div class="line"></div>
      <div class="bill">
        <h5>发票信息：</h5>
        <div>普通发票（电子） 个人 明细</div>
        <h5>使用优惠/抵用</h5>
      </div>
    </div>
    <div class="money clearFix">
      <ul>
        <li>
          <b
            ><i>{{ orderList.totalNum }}</i
            >件商品，总商品金额</b
          >
          <span>¥{{ orderList.totalAmount }}</span>
        </li>
        <li>
          <b>返现：</b>
          <span>0.00</span>
        </li>
        <li>
          <b>运费：</b>
          <span>0.00</span>
        </li>
      </ul>
    </div>
    <div class="trade">
      <div class="price">
        应付金额:　<span>¥{{ orderList.totalAmount }}.00</span>
      </div>
      <div class="receiveInfo">
        寄送至:
        <span>{{ userDefaultAddr.fullAddress }}</span>
        收货人：<span>{{ userDefaultAddr.consignee }}</span>
        <span>{{ userDefaultAddr.phoneNum }}</span>
      </div>
    </div>
    <div class="sub clearFix">
      <!-- <router-link class="subBtn" to="/pay" @click="submitOrder"
        >提交订单</router-link
      > -->
      <a class="subBtn" @click="submitOrder">提交订单</a>
    </div>
  </div>
</template>

<script>
import { mapState, mapGetters } from "vuex";
export default {
  name: "Trade",
  data() {
    return {
      msg: "",
      orderId: "",
    };
  },
  computed: {
    ...mapState("trade", ["addrList", "orderList"]),
    ...mapGetters("trade", ["detailOrderList"]),
    // 默认地址显示
    userDefaultAddr() {
      // find查找数组当中符合条件的元素返回(此处返回的是一个对象)
      return this.addrList.find((item) => item.isDefault == 1) || {};
    },
  },
  mounted() {
    try {
      this.$store.dispatch("trade/getAddrList");
    } catch (error) {
      alert(error.message);
    }
    try {
      this.$store.dispatch("trade/getOrderList");
    } catch (error) {
      alert(error.message);
    }
  },
  methods: {
    changeDefault(addr) {
      // 先将全部改为0
      this.addrList.forEach((item) => {
        item.isDefault = 0;
      });
      // 再将点中的地址改为1
      addr.isDefault = 1;
    },
    async submitOrder() {
      // 交易编号
      let tradeNo = this.orderList.tradeNo;
      // 交易信息
      let data = {
        consignee: this.userDefaultAddr.consignee,
        consigneeTel: this.userDefaultAddr.phoneNum,
        deliveryAddress: this.userDefaultAddr.fullAddress,
        paymentWay: "ONLINE",
        orderComment: this.msg,
        orderDetailList: this.detailOrderList,
      };
      // 使用全局API--提交订单请求--返回订单号
      let result = await this.$API.reqSubmitOrder(tradeNo, data);
      console.log("submitOrder success", result);
      if (result.code == 200) {
        // 提交订单成功--存储订单号
        this.orderId = result.data;
        // 路由跳转+路由传参
        this.$router.push(`/pay?orderId=${this.orderId}`);
      } else {
        // 提交订单失败
        alert(result.data);
      }
    },
  },
};
</script>

<style lang="less" scoped>
.trade-container {
  .title {
    width: 1200px;
    margin: 0 auto;
    font-size: 14px;
    line-height: 21px;
  }

  .content {
    width: 1200px;
    margin: 10px auto 0;
    border: 1px solid rgb(221, 221, 221);
    padding: 25px;
    box-sizing: border-box;
    background-color: #fffeee;

    .receive,
    .pay {
      line-height: 36px;
      margin: 18px 0;
    }

    .address {
      padding-left: 20px;
      margin-bottom: 15px;

      .username {
        float: left;
        width: 100px;
        height: 30px;
        line-height: 30px;
        text-align: center;
        border: 1px solid #ddd;
        position: relative;
      }

      .username::after {
        content: "";
        display: none;
        width: 13px;
        height: 13px;
        position: absolute;
        right: 0;
        bottom: 0;
        background: url(./images/choosed.png) no-repeat;
      }

      .username.selected {
        border-color: #734931;
      }

      .username.selected::after {
        display: block;
      }

      p {
        width: 610px;
        float: left;
        line-height: 30px;
        margin-left: 10px;
        padding-left: 5px;
        cursor: pointer;

        .s1 {
          float: left;
        }

        .s2 {
          float: left;
          margin: 0 5px;
        }

        .s3 {
          float: left;
          width: 56px;
          height: 24px;
          line-height: 24px;
          margin-left: 10px;
          background-color: #734931;
          color: #fffeee;
          margin-top: 3px;
          text-align: center;
        }
      }

      p:hover {
        background-color: #ddd;
      }
    }

    .line {
      height: 1px;
      background-color: #ddd;
    }

    .way {
      width: 1080px;
      height: 110px;
      background: #734931;
      padding: 8px;
      margin: 0 auto;
      color: #fffeee;

      h5 {
        line-height: 50px;
      }

      .info {
        margin-top: 20px;

        .s1 {
          float: left;
          border: 1px solid #ddd;
          width: 120px;
          height: 30px;
          line-height: 30px;
          text-align: center;
          margin-right: 10px;
        }

        p {
          line-height: 30px;
        }
      }
    }

    .detail {
      width: 1080px;
      border: 2px solid #734931;
      // background: #897974;
      padding: 15px;
      margin: 2px auto 0;

      h5 {
        line-height: 50px;
      }

      .list {
        display: flex;
        justify-content: space-between;

        li {
          line-height: 30px;

          p {
            margin-bottom: 20px;
          }

          h4 {
            color: #734931;
            font-weight: 400;
          }

          h3 {
            color: #734931;
          }
        }
      }
    }

    .bbs {
      margin-bottom: 15px;

      h5 {
        line-height: 50px;
      }

      textarea {
        width: 100%;
        border-color: #e4e2e2;
        line-height: 1.8;
        outline: none;
        resize: none;
        background-color: rgb(255, 255, 255);
      }
    }

    .bill {
      h5 {
        line-height: 50px;
      }

      div {
        padding-left: 15px;
      }
    }
  }

  .money {
    background-color:#fffeee;
    width: 1200px;
    margin: 20px auto;
    padding: 20px;

    ul {
      width: 220px;
      float: right;

      li {
        line-height: 30px;
        display: flex;
        justify-content: space-between;

        i {
          color: #734931;
        }
      }
    }
  }

  .trade {
    box-sizing: border-box;
    width: 1200px;
    padding: 10px;
    margin: 10px auto;
    text-align: right;
    background-color: #734931;
    border: 1px solid #ddd;
    color: #fffeee;

    div {
      line-height: 30px;
    }

    .price span {
      color: #fffeee;
      font-weight: 700;
      font-size: 14px;
    }

    .receiveInfo {
      color: #fffeee;
    }
  }

  .sub {
    width: 1200px;
    margin: 0 auto 10px;

    .subBtn {
      float: right;
      width: 164px;
      height: 56px;
      font: 700 18px "微软雅黑";
      line-height: 56px;
      text-align: center;
      color: #fffeee;
      background-color: #734931;
    }
    .subBtn:hover{
      background-color: #391e0f;
      color: #fffeee;
    }
  }
}
</style>