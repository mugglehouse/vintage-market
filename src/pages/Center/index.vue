<template>
  <div class="order-main">
    <div class="container">
      <div class="order-body">
        <!--左侧列表-->
        <div class="order-left">
          <dl>
            <dt><i>·</i> 订单中心</dt>
            <dd>
              <router-link to="/center/myorder">我的订单</router-link>
            </dd>
            <dd>
              <router-link to="/center/grouporder">团购订单</router-link>
            </dd>
          </dl>
          <dl>
            <dt><i>·</i> 设置</dt>
            <dd>个人信息</dd>
            <dd>收货地址</dd>
          </dl>
        </div>
        <!-- 右侧内容 -->
        <!-- <MyOrder /> -->
        <router-view></router-view>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "Center",
 
};
</script>

<style lang="less">
.order-main {
  .container {
    margin: 0 auto;
    width: 1200px;

    .order-body {
      padding: 10px;
      color: #333;

      &:after {
        content: "";
        display: block;
        clear: both;
      }

      //左边
      .order-left {
        float: left;
        width: 16.67%;

        dl {
          line-height: 28px;

          dt {
            font-weight: 700;
            padding: 5px;

            i {
              color: #77b72c;
            }
          }

          dd {
            margin: 0 0 6px;
            border-bottom: 1px solid #ededed;
            text-align: center;
          }
        }
      }

      //右边
      .order-right {
        float: right;
        width: 83.33%;

        //订单部分
        .order-content {
          margin: 0 20px;
          color: #666;

          //标题
          .title {
            margin-bottom: 22px;
            border: 1px solid #ddd;

            h3 {
              padding: 12px 10px;
              font-size: 15px;
              color: #734931;
              background-color: #fffeee;
            }
          }

          //表头
          .chosetype {
            margin-bottom: 15px;
            color: #734931;

            table {
              border: 1px solid #e6e6e6;
              border-collapse: separate;
              border-radius: 2px;
              width: 100%;
              max-width: 100%;
              border-spacing: 0;

              th {
                padding: 6px 8px;
                color: #fffeee;
                font-weight: 700;
                vertical-align: bottom;
                background-color: #734931;
                line-height: 18px;
                border-bottom: 1px solid #e6e6e6;
                font-size: 12px;
                text-align: left;
              }
            }
          }

          // 表单内容
          .orders {
            font-size: 12px;
            background-color: #fffeee;

            a {
              &:hover {
                color: #4cb9fc;
              }
            }

            table {
              border: 1px solid #e6e6e6;
              border-collapse: collapse;
              border-radius: 2px;
              width: 100%;
              margin-bottom: 18px;
              max-width: 100%;

              th {
                padding: 6px 8px;
                line-height: 18px;
                text-align: left;
                vertical-align: bottom;
                background-color: #734931;
                font-size: 12px;
                color: #fffeee;

                .pull-right {
                  float: right;
                  cursor: pointer;

                  img {
                    margin-right: 10px;
                    vertical-align: middle;
                  }
                }
              }

              td {
                font-size: 12px;
                color: #666;
                padding: 6px 8px;
                line-height: 18px;
                text-align: left;
                vertical-align: middle;
                border: 1px solid #e6e6e6;

                &.center {
                  text-align: center;
                }

                .typographic {
                  img {
                    float: left;
                    margin-right: 10px;
                  }

                  a {
                    float: left;
                    min-width: 80px;
                    max-width: 250px;
                    color: #734931;

                    &.service {
                      color: #666;
                    }
                  }

                  span {
                    float: left;
                    min-width: 80px;
                    max-width: 250px;
                    text-align: center;
                  }
                }
              }
            }
          }

          // 分页
          .choose-order {
            .pagination {
              margin: 38px 0;

              ul {
                font-size: 0;
                display: inline-block;

                li {
                  display: inline-block;
                  padding: 4px 9px;
                  font-size: 14px;
                  border: 1px solid #e0e9ee;

                  &.prev,
                  &.next {
                    background-color: #fffeee;
                  }

                  &.prev {
                    border-right-color: #28a3ef;
                  }

                  &.page {
                    border-color: #28a3ef;
                    border-left: 0;

                    &.actived {
                      background-color: #28a3ef;

                      a {
                        color: #fff;
                      }
                    }
                  }
                }
              }

              div {
                display: inline-block;
                font-size: 14px;
                color: #333;
              }
            }
          }
        }

        // 猜你喜欢
        .like {
          border: 1px solid #ddd;
          margin: 15px 20px;
          background-color: #fffeee;

          .kt {
            border-bottom: 1px solid #ddd;
            background: #734931;
            color: #fffeee;
            margin: 0;
            padding: 12px;
            font-size: 15px;
          }

          .like-list {
            padding: 15px 11px;
            overflow: hidden;

            .likeItem {
              width: 25%;
              float: left;

              .p-img {
                text-align: left;

                img {
                  width: 167px;
                  height: 123px;
                }
              }

              .attr {
                padding: 0 15px;
              }

              .price {
                padding: 0 15px;
                font-size: 16px;
                color: #734931;
                margin-bottom: 20px;
              }

              .commit {
                padding: 0 15px;
              }
            }
          }
        }
      }
    }
  }
}
</style>