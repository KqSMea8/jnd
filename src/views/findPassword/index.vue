<template>
  <div style="height: 100%;">
    <div class="header clearfix">
      <div class="fll login" @click="goLogin">登录</div>
      <div class="fll help" @click="goHelp">帮助中心</div>
    </div>
    <div class="main-find">
      <div class="forget-wrap">
        <div class="title">修改密码</div>
        <Step :list="steps" :active="activeStep"></Step>
        <div class="content">
          <div v-if="activeStep === 1">
            <div class="select_type">
              <div class="phone" @click="setPhone"></div>
              <span class="type_text">手机号修改密码</span>
            </div>
            <!--<div class="select_type">-->
            <!--<div class="email" @click="setEmail"></div>-->
            <!--<span class="type_text">邮箱修改密码</span>-->
            <!--</div>-->
          </div>
          <TypeInput
            v-if="activeStep === 2"
            :type="type"
            @error="goFirstStep"
            @success="handleSubmit"
          ></TypeInput>
          <div v-if="activeStep === 3" class="three_step">
            <p>请设置新密码：</p>
            <input type="password" @keyup="checkPassword" v-model="password">
            <div class="level clearfix" v-if="level > 0">
              <div :class="level >= 1 ? 'level-item item' : 'level-item' "></div>
              <div :class="level >= 2 ? 'level-item item' : 'level-item' "></div>
              <div :class="level === 3 ? 'level-item item' : 'level-item' "></div>
            </div>
            <span
              class="level-text"
            >{{level === 1 ? '弱' : level === 2 ? '中' : level === 3 ? '强' : ''}}</span>
            <div class="pwd-wran" v-if="password.length < 6">密码长度6~10位，字母数字组合</div>
            <p>再次输入新密码：</p>
            <input type="password" v-model="newPassword">
            <br>
            <button @click="resetPassword">确认</button>
          </div>
          <div v-if="activeStep === 4">
            <div class="success"></div>
            <p class="success_text">重置密码成功</p>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Step from "../../component/step";
import TypeInput from "../../component/inputType";

export default {
  name: "index",
  data() {
    return {
      steps: ["选择找回方式", "身份验证", "设置密码", "完成"],
      activeStep: 1,
      type: "",
      password: "",
      newPassword: "",
      num: "",
      level: 0 //密码等级
    };
  },
  components: {
    Step,
    TypeInput
  },
  methods: {
    goLogin() {
      this.$router.push({ path: "/home", query: { login: 1 } });
    },
    goHelp() {
      this.$router.push("/help/guide");
    },

    setPhone() {
      this.activeStep++;
      this.type = "phone";
    },
    // setEmail () {
    //   this.activeStep++
    //   this.type = 'email'
    // },
    // 填写成功
    handleSubmit(val) {
      this.num = val;
      this.activeStep++;
    },
    // 不填写
    goFirstStep() {
      this.activeStep = 1;
    },
    //校验密码
    checkPassword() {
      let checkChar = /[a-zA-Z]/g;
      let checkNum = /[0-9]/g;
      if (this.password.length >= 6) {
        if (checkChar.test(this.password) && checkNum.test(this.password)) {
          this.level = 2;
          if (this.password.length > 8) this.level = 3;
        } else if (
          (checkChar.test(this.password) || checkNum.test(this.password)) &&
          this.password.length > 8
        ) {
          this.level = 2;
        }
      } else if (this.password.length > 0) {
        this.level = 1;
      } else {
        this.level = 0;
      }
    },
    // 重置密码
    resetPassword() {
      if (this.password.length >= 6 && this.password.length <= 10) {
        if (this.password === this.newPassword) {
          this.$axios
            .get(
              `user/forgetPassword/${this.num}/${this.password}/${this.level}`
            )
            .then(res => {
              if (res.status === 200) {
                setTimeout(() => {
                  this.activeStep++;
                }, 1000);
              } else {
                this.$message.warning(res.msg);
                this.newPassword = "";
                this.password = "";
              }
            });
        } else {
          this.$message.warning("两次密码不一致");
        }
      } else {
        this.$message.warning("密码长度设置不规范");
      }
    }
  }
};
</script>

<style scoped lang="scss">
.header {
  width: 100%;
  height: 32px;
  background: rgba(249, 249, 249, 1);
  .login {
    font-size: 14px;
    font-family: PingFangSC-Regular;
    font-weight: 400;
    color: rgba(155, 155, 155, 1);
    line-height: 32px;
    margin-left: 80%;
    cursor: pointer;
  }
  .help {
    font-size: 14px;
    font-family: PingFangSC-Regular;
    font-weight: 400;
    color: rgba(155, 155, 155, 1);
    line-height: 32px;
    margin-left: 40px;
    cursor: pointer;
  }
}
.main-find {
  width: 100%;
  height: 100%;
  padding-bottom: 0;
  padding-top: 30px;
  background: rgba(245, 245, 245, 1);
}
.forget-wrap {
  margin: auto;
  text-align: center;
  box-sizing: border-box;
  border: 1px solid transparent;
  width: 900px;
  height: 394px;
  background: rgba(255, 255, 255, 1);

  .title {
    margin-top: 24px;
    margin-left: 56px;
    width: 64px;
    height: 16px;
    font-size: 16px;
    font-family: PingFangSC-Regular;
    font-weight: 400;
    color: rgba(81, 81, 81, 1);
    line-height: 16px;
  }

  .content {
    width: 100%;
    height: 50%;
    margin-top: 60px;
    .select_type {
      display: inline-block;
      margin: 0 30px;
      vertical-align: middle;
      border: none;
      cursor: pointer;
      outline: none;
      text-align: center;
      .type_text {
        display: inline-block;
        margin-top: 10px;
        font-size: 14px;
        font-family: PingFangSC-Regular;
        font-weight: 400;
        color: rgba(208, 172, 86, 1);
        line-height: 14px;
      }
    }

    .phone {
      width: 48px;
      height: 48px;
      border-radius: 50%;
      margin: auto;
      background-image: url("./imgs/phone_normal.svg");
    }
    .phone:hover {
      background: url("./imgs/phone_active.svg");
    }
    .email {
      width: 48px;
      height: 48px;
      border-radius: 50%;
      margin: auto;
      background: url("./imgs/email_normal.svg");
    }
    .email:hover {
      background: url("./imgs/email_active.svg");
    }
    .success {
      margin: auto;
      width: 40px;
      height: 40px;
      background: url("./imgs/success.svg");
    }
    .success_text {
      margin: auto;
      width: 84px;
      height: 40px;
      font-size: 14px;
      font-family: PingFangSC-Regular;
      font-weight: 400;
      color: rgba(155, 155, 155, 1);
      line-height: 40px;
    }
    .three_step {
      margin: auto;
      width: 160px;
      text-align: left;
      white-space: nowrap;
      p {
        height: 14px;
        font-size: 14px;
        font-family: PingFangSC-Regular;
        font-weight: 400;
        color: rgba(155, 155, 155, 1);
        line-height: 14px;
      }
      input {
        margin: 10px 0;
        padding-left: 5px;
        width: 160px;
        height: 30px;
        border-radius: 2px;
        font-size: 16px;
        border: 1px solid rgba(155, 155, 155, 1);
      }
      button {
        width: 60px;
        height: 30px;
        background: rgba(208, 172, 86, 1);
        outline: none;
        border: none;
        color: #fff;
        cursor: pointer;
      }
      .level {
        display: inline-block;

        .level-item {
          float: left;
          margin-left: 2px;
          width: 25px;
          height: 8px;
          border: 1px solid #eee;
        }
        .item {
          background: #0f0;
        }
      }
      .level-text {
        font-size: 14px;
        color: #666;
        margin-left: 5px;
      }
      .pwd-wran {
        font-size: 14px;
        color: #a80e0e;
        margin-bottom: 10px;
      }
    }
  }
}
</style>
