<template>
  <div class="bottom-tap">
    <div class="container">
      <div class="info-wrap">
        <div class="left">
          <div class="about">
            <h3>关于我们</h3>
            <p class="container">9能贷联合各类大型的金融机构，深度挖掘安全的金融服务渠道机构，为用户提供安全、多元的贷款产品和定制化的专业资金服务，打造更契合用户自身需求的贷款管理方案。</p>
            <p class="container">我们将人文情怀融入金融服务之中，为用户创造更多价值，为社会营造诚信健康的金融环境，有效推动普惠金融发展。</p>
          </div>
          <div class="business-type">
            <h3>业务项目</h3>
            <p class="container" @click="$router.push(`/agent`)">找经纪人</p>
            <p class="container" @click="$router.push(`/organization`)">找机构</p>
            <p class="container" @click="$router.push(`/productList`)">找贷款产品</p>
          </div>
        </div>
        <div class="center">
          <div>扫一扫，使用小程序</div>
          <div class="pic">
            <img src="/static/resource/1.jpg" alt="">
          </div>
        </div>
        <div class="right">
          <div class="loan-guide">
            <h3>贷款指南</h3>
            <p class="container" @click="$router.push(`/help/guide`)">如何办理贷款</p>
            <p class="container" @click="$router.push(`/help/matter`)">资质相关问题</p>
            <p class="container" @click="$router.push(`/help/question`)">相关注意事项</p>
          </div>
          <div class="contact-us">
            <h3>联系我们</h3>
            <p class="container">市场合作：1415929656@qq.com</p>
            <p class="container">机构合作：1415929656@qq.com</p>
          </div>
        </div>
      </div>

      <div class="copy-right">
        <p>9能贷-信息平台 版权所有 Copyright @2019 www.9nengdai.com</p>
        <p>地址：北京市 朝阳区 嘉美中心写字楼1620</p>
        <p class="copy-info">
          <a href="http://www.beian.gov.cn/portal/registerSystemInfo?recordcode=11010502037638" target="_blank"><i></i>京公网安备11010502037638号</a>
        </p>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "bottomTap"
}
</script>

<style scoped lang="scss">
$textColor: #A3A3A9;
.bottom-tap {
  width:100%;
  height:270px;
  background:rgba(255,255,255,1);
  z-index: 9;
  div.container {
    width: 1200px;
    margin: 0 auto;
    padding: 20px 100px 8px;
    box-sizing: border-box;
    position: relative;
    color:#515151;
    font-size: 14px;
    .info-wrap {
      overflow: hidden;
      h3 {
        font-size: 14px;
        padding: 6px 0;
        position: relative;
        &::after {
          content: '';
          position: absolute;
          bottom: 0;
          left: 0;
          width: 4em;
          height: 1px;
          background: #515151;
        }
      }
      p.container {
        font-size: 12px;
        margin: 10px 0;
        line-height: 1.5;
        color: $textColor;
      }
      .left {
        width: 400px;
        float: left;
        .about {
          width: 220px;
          float: left;
        }
        .business-type {
          float: right;
          margin-right: 60px;
          p {
            cursor: pointer;
          }
        }
      }
      .center {
        position: absolute;
        left: 500px;
        right: 500px;
        text-align: center;
        .pic {
          width:140px;
          height: 140px;
          margin: 20px auto;
          img {
            width: 100%;
          }
        }
      }
      .right {
        float: right;
        width: 400px;
        .loan-guide {
          float: left;
          margin-left: 60px;
          cursor: pointer;
        }
        .contact-us {
          float: right;
        }
      }
    }
    .copy-right {
      text-align: center;
      p {
        margin: 4px 0;
      }
      .copy-info {
        padding-top: 6px;
        height: 16px;
        line-height: 16px;
        a {
          i {
            display: inline-block;
            width: 14px;
            height: 16px;
            margin-right: 6px;
            background: url('../../static/copyright.png');
            vertical-align: middle;
            margin-top: -4px;
          }
        }
      }
    }

  }
}
</style>
