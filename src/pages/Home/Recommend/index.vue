<template>
  <div>
    <!--今日推荐-->
    <div class="today-recommend">
      <div class="py-container">
        <ul class="recommend">
          <li class="clock">
              <h4>今日推荐</h4>
            <div class="time">
              <img src="./images/clock.png" />
            </div>
          </li>
          <li class="banner">
            <img src="./images/today01.png" />
          </li>
          <li class="banner">
            <img src="./images/today02.png" />
          </li>
          <li class="banner">
            <img src="./images/today03.png" />
          </li>
          <li class="banner">
            <img src="./images/today04.png" />
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "Recommend",
};
</script>

<style lang="less" scoped>
.today-recommend {
  .py-container {
    width: 1200px;
    margin: 30px auto;

    .recommend {
      height: 165px;
      background-color: #eaeaea;
      margin: 10px 0;
      display: flex;

      .clock {
        width: 16.67%;
        background-color: #734931;
        color: #fff;
        font-size: 18px;
        text-align: center;

        

        .time {
          img{
            width: 100%;
            // height: 80%;
          }
        }

        h4 {
          margin: 1px 0;
          font-weight: 700;
          font-size: 18px;
          line-height: 30.06px;
        }
      }

      .banner {
        width: 20.83%;

        img {
          width: 100%;
          height: 100%;
          transition: all 400ms;

          &:hover {
            opacity: 0.8;
          }
        }
      }
    }
  }
}
</style>