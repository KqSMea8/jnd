<template>
  <div class="home">
    <div class="img">
      <el-carousel trigger="click">
        <el-carousel-item v-for="(item, index) in bannerList" :key="item" @click.native="handleBanner(index)">
          <div :style="{ width: 100 + '%', height: 448 + 'px', background: 'url(' + item + ') center center no-repeat' }"></div>
        </el-carousel-item>
      </el-carousel>
      <div class="apply">
        <div class="title">贷款申请</div>
        <el-form :model="borrowerData" ref="borrowerData" :rules="borrowerDataRules" style="margin-left: 60px;">
          <el-form-item prop="borrowerName">
            <el-input placeholder="请输入姓名" v-model="borrowerData.borrowerName" :disabled="isUpdate" class="name-input"></el-input>
            <el-radio-group v-model="borrowerData.sex" :disabled="isUpdate" style="margin-left: 10px;">
              <el-radio :label="1">男</el-radio>
              <el-radio :label="0">女</el-radio>
            </el-radio-group>
          </el-form-item>
          <el-form-item class="input-item" prop="loanAmount">
            <el-input placeholder="贷款金额（万元）" v-model="borrowerData.loanAmount"></el-input>
          </el-form-item>

          <el-form-item prop="address">
            <el-select
              v-model="borrowerData.address1"
              style="width: 120px;float: left"
              placeholder="地区"
              @change="getCity"
              clearable
            >
              <el-option
                v-for="item in provinceData"
                :key="item.pid"
                :label="item.provincial"
                :value="item.pid"
                style="width: 140px;"
              ></el-option>
            </el-select>
            <el-select
              v-model="borrowerData.address2"
              style="margin-left: 16px;width: 120px;float: left;"
              placeholder="地区"
              clearable
            >
              <el-option
                style="width: 140px;"
                v-for="item in cityData"
                :key="item.cid"
                :label="item.city"
                :value="item.cid"
              ></el-option>
            </el-select>
          </el-form-item>
          <el-form-item class="input-item" prop="phone">
            <el-input type="text" v-model="borrowerData.phone" placeholder="手机号" :disabled="isUpdate"></el-input>
          </el-form-item>
          <el-form-item prop="code">
            <el-input
              type="text"
              v-model="borrowerData.code"
              placeholder="验证码"
              class="name-input"
            ></el-input>
            <el-button class="name-input send" v-if="showing" @click="send">{{verifyCode}}</el-button>
            <el-button class="name-input send" v-else>{{time}}s</el-button>
          </el-form-item>
          <el-form-item>
            <el-button @click="apply" style="width: 212px; margin-left: 30px;">立即申请</el-button>
          </el-form-item>
        </el-form>
        <div class="agreement">
          <el-checkbox v-model="isChecked"></el-checkbox>
         <el-dropdown trigger="click">
          <span class="el-dropdown-link">
            阅读并同意9能贷用户相关协议<i class="el-icon-arrow-down el-icon--right"></i>
          </span>
          <el-dropdown-menu slot="dropdown">
            <el-dropdown-item icon="el-icon-plus"><a href="#/agreement?userProtect" target="_blank">《9能贷用户隐私保护政策》</a></el-dropdown-item>
            <el-dropdown-item icon="el-icon-circle-plus"><a href="#/agreement?userRegister" target="_blank">《9能贷用户注册协议》</a></el-dropdown-item>
          </el-dropdown-menu>
        </el-dropdown>
        </div>
      </div>
    </div>
    <div class="wrap pt48">
      <div class="w1200">
        <div class="wrap-top clearfix mb24 ml111">
          <div class="clearfix fll single">
            <img src="../../static/resource/group9.png" alt="" class="fll">
            <div class="fll common">
              <div class="text1">放款快</div>
              <div class="text2">最快2小时放款</div>
            </div>
          </div>
          <div class="clearfix fll single ml217">
            <img src="../../static/resource/Group7.png" alt="" class="fll">
            <div class="fll common">
              <div class="text1">利率低</div>
              <div class="text2">利率低至0.26%</div>
            </div>
          </div>
          <div class="clearfix fll single ml227">
            <img src="../../static/resource/Group3.png" alt="" class="fll">
            <div class="fll common">
              <div class="text1">额度高</div>
              <div class="text2">最高可贷1000万</div>
            </div>
          </div>
        </div>
        <div class="wrap-bottom clearfix ml111">
          <div class="clearfix fll single">
            <img src="../../static/resource/Group.png" alt="" class="fll">
            <div class="fll common">
              <div class="text1">门槛低</div>
              <div class="text2">月入2000即可贷</div>
            </div>
          </div>
          <div class="clearfix fll single ml208">
            <img src="../../static/resource/Group11.png" alt="" class="fll">
            <div class="fll common">
              <div class="text1">渠道广</div>
              <div class="text2">多种贷款方式供您选择</div>
            </div>
          </div>
          <div class="clearfix fll single ml188">
            <img src="../../static/resource/Group8.png" alt="" class="fll">
            <div class="fll common">
              <div class="text1">服务好</div>
              <div class="text2">资深顾问在线为您解答</div>
            </div>
          </div>
        </div>
        <div class="special mt48 mb24">特别推荐</div>
        <div class="clearfix special-wrap">
          <div class="fll com mr20" v-for="(item,index) in specialData" :key="index">
            <div class="word1">{{item.productType}}</div>
            <div>
              <div class="word2">{{item.productPublisher}}</div>
            </div>
            <div class="word3">
              {{item.productInterest}}%
              <!-- <span class="symbol">%</span> -->
            </div>
            <div class="word4">利息</div>
            <div class="word5">{{item.productStartAmount}}-{{item.productEndAmount}}万</div>
            <div class="btn" @click="specialLook(item.productId)">立即查看</div>
          </div>
        </div>
        <div class="main clearfix">
          <div class="fll">
            <div class="clearfix mt48 mb24">
              <div class="product fll">产品推荐</div>
              <div class="fll">
                <div class="more fll" @click="$router.push('/loans')">更多</div>
                <img src="../../static/resource/right.png" alt="" class="fll img-right">
              </div>
            </div>
            <div class="clearfix">
              <div class="product-wrap fll align">
                <div class="mark2">
                  <div class="agent-write">优质产品 多样选择</div>
                  <div class="agent-btn" @click="$router.push('/Loans')">立即查看</div>
                </div>
              </div>
              <div class="product-table fll">
                <div class="product-item" v-for="(item,index) in productData" :key="index">
                  <div class="clearfix">
                    <div class="fll">
                      <div class="item1-num">{{item.productName}}</div>
                      <div class="item-same first">产品名称</div>
                    </div>
                    <div class="item1 fll align">
                      <div class="item-num">
                        {{item.productInterest}}%
                        <span class="percent"></span>
                      </div>
                      <div class="item-same">利息</div>
                    </div>
                    <div class="item1 fll align">
                      <div class="item-num">{{item.producLoanLength}}</div>
                      <div class="item-same">放款时间</div>
                    </div>
                    <div class="item1 fll align">
                      <div class="item-num">{{item.productLife}}</div>
                      <div class="item-same">使用年限</div>
                    </div>
                    <div class="item1 fll align">
                      <div class="item-num">
                        {{item.productStartAmount}}万
                        <span class="percent"></span>
                      </div>
                      <div class="item-same">起贷金额</div>
                    </div>
                    <div class="flr organ-btn align" @click="proDetail(item.productId)">立即查看</div>
                  </div>
                </div>
              </div>
            </div>
            <div class="clearfix mt48 mb24">
              <div class="agent fll">经纪人推荐</div>
              <div class="fll">
                <div class="more-agent fll" @click="$router.push('/agent')">更多</div>
                <img src="../../static/resource/right.png" alt="" class="fll img-right">
              </div>
            </div>
            <div class="clearfix">
              <div class="agent-wrap fll align">
                <div class="mark3">
                  <div class="agent-write">资深人士 专业服务</div>
                  <div class="agent-btn" @click="$router.push('/agent')">立即查看</div>
                </div>
              </div>
              <div class="agent-table fll">
                <div class="clearfix">
                  <div class="fll align mr42" v-for="(item,index) in agentData" :key="index">
                    <img
                      v-if="item.image"
                      :src="item.image"
                      style="width: 100px;height: 98px;cursor: pointer"
                      @click="agDetail(item.brokerId)"
                    >
                    <img
                      v-else
                      src="/static/resource/pic/agent.png"
                      style="width: 100px;height: 98px;cursor: pointer"
                      @click="agDetail(item.brokerId)"
                    >
                    <div class="agent-name">{{item.brokerName}}</div>
                    <div class="agent-job">金融顾问</div>
                  </div>
                </div>
              </div>
            </div>
            <div class="clearfix mt48 mb24">
              <div class="organize fll">机构推荐</div>
              <div class="fll">
                <div class="more fll" @click="$router.push('/organization')">更多</div>
                <img src="../../static/resource/right.png" alt="" class="fll img-right">
              </div>
            </div>
            <div class="clearfix">
              <div class="organize-wrap fll align">
                <div class="mark1">
                  <div class="agent-write">权威机构 财富保障</div>
                  <div class="agent-btn" @click="$router.push('/Organization')">立即查看</div>
                </div>
              </div>
              <div class="organize-table fll">
                <div class="clearfix">
                  <div class="empty-list" v-if="organData.length === 0">
                    <img :src="emptyList" alt="">
                    <p>暂无数据...</p>
                  </div>
                  <div class="fll organW" v-for="(item,index) in organData" :key="index">
                    <img
                      v-if="item.agencyLogo"
                      :src="item.agencyLogo"
                      alt=""
                      style="width: 100px;height: 98px;cursor: pointer"
                      @click="orDetail(item.agencyId)"
                    >
                    <img
                      v-else
                      src="/static/resource/pic/organ.png"
                      alt=""
                      style="width: 100px;height: 98px;cursor: pointer"
                      @click="orDetail(item.agencyId)"
                    >
                    <div class="organ-text1">{{item.agencyName}}</div>
                    <div class="organ-text2">{{item.agencyIntroduction}}</div>
                  </div>
                </div>
              </div>
            </div>
          </div>
          <div class="fll mt48">
            <div class="loans clearfix align">
              <div class="loans-use">贷款用途</div>
              <div class="loans-left fll mr60">
                <img src="../../static/resource/Group16Copy.png" alt="">
                <div class="loans-text">装修贷款</div>
                <img src="../../static/resource/Group20Copy.png" alt="">
                <div class="loans-text">装修贷款</div>
                <img src="../../static/resource/Group21Copy.png" alt="">
                <div class="loans-text">装修贷款</div>
                <img src="../../static/resource/Group27Copy.png" alt="">
                <div class="loans-text">装修贷款</div>
              </div>
              <div class="loans-right fll">
                <img src="../../static/resource/Group17Copy.png" alt="">
                <div class="loans-text">装修贷款</div>
                <img src="../../static/resource/Group19Copy.png" alt="">
                <div class="loans-text">装修贷款</div>
                <img src="../../static/resource/Group22Copy.png" alt="">
                <div class="loans-text">装修贷款</div>
                <img src="../../static/resource/icon.png" alt="">
                <div class="loans-text">装修贷款</div>
              </div>
            </div>
            <div class="message clearfix">
              <img src="../../static/resource/laba.png" alt="" class="fll">
              <div class="fll mess-main">
                <vue-seamless-scroll :data="loanData" class="seamless-warp" :class-option="defaultOption">
                  <div
                    class="mess-text"
                    :key="index"
                    v-for="(item,index) in loanData"
                  >{{item.allName}}:{{item.phoneG}}&nbsp;&nbsp;申请了贷款</div>
                </vue-seamless-scroll>
              </div>
            </div>
          </div>
        </div>
        <footerSame></footerSame>
      </div>
    </div>
    <bottomTap></bottomTap>
  </div>
</template>

<script>
import footerSame from "../component/footerSame";
import bottomTap from "../component/bottomTap";
import validateApplyApi from "./api/validateApply";
import publicApi from "./api/public";
import emptyList from "../assets/empty-list.png";
import { validaterPhone, validaterLoanAmount, validaterName } from "@/util/validate";
import banner01 from '@/assets/banner01.png'
import banner02 from '@/assets/banner02.png'
import banner03 from '@/assets/banner03.png'
const bannerList = [banner01, banner02, banner03]
export default {
  name: "Home",
  components: {
    footerSame,
    bottomTap
  },
  data() {
    const validateLoanAmount = (rule, value, callback) => {
      if (value) {
        if (validaterLoanAmount(value)) {
          callback();
        } else {
          callback(new Error("贷款金额格式错误"));
        }
      } else {
        callback(new Error("贷款金额不能为空"));
      }
    };
    const validateAddress = (rule, value, callback) => {
      if (this.borrowerData.address1 && this.borrowerData.address2) {
        callback();
      } else {
        callback(new Error("请选择地址"));
      }
    };
    const validatePhone = (rule, value, callback) => {
      if (value) {
        if (validaterPhone(value)) {
          callback();
        } else {
          callback(new Error("手机号格式错误"));
        }
      } else {
        callback(new Error("手机号不能为空"));
      }
    };
    const validateName = (rule, value, callback) => {
      if (value) {
        if (validaterName(value)) {
          callback();
        } else {
          callback(new Error("姓名格式错误"));
        }
      } else {
          callback(new Error("姓名不能为空"));
      }
    };
    return {
      defaultOption: {
        step: 0.4,
        openWatch: true,
        waitTime: 1000
      },
      isChecked: true,
      bannerList,
      emptyList,
      showing: true,
      time: 60,
      verifyCode: "发送验证码",
      borrowerData: {
        borrowerName: "",
        sex: 1,
        loanAmount: "",
        address1: "",
        address2: "",
        phone: "",
        code: ""
      },
      borrowerDataRules: {
        code: [{ required: true, message: '验证码不能为空', trigger: "change" }],
        borrowerName: [
          { required: true, trigger: "change", validator: validateName }
        ],
        loanAmount: [
          { required: true, trigger: "change", validator: validateLoanAmount }
        ],
        address: [
          { required: true, trigger: "change", validator: validateAddress }
        ],
        phone: [{ required: true, trigger: "change", validator: validatePhone }]
      },
      //省
      provinceData: [],
      //市 区
      cityData: [],
      specialData: [],
      productData: [],
      agentData: [],
      organData: [],
      loanData: [],
      timer: null,
      isUpdate: false
    };
  },
  computed: {
    getUser() {
      return this.$store.state.userInfo
    }
  },
  watch: {
    getUser(val) {
      if (val) {
        this.borrowerData.borrowerName = val.name
        this.borrowerData.sex = val.sex
        this.borrowerData.phone = val.phone
        this.isUpdate = true
      } else {
        this.borrowerData.borrowerName = ''
        this.borrowerData.phone = ''
        this.isUpdate = false
      }
    }
  },
  created() {
    let time = sessionStorage.getItem('time')
    if (time > 0) {
      this.timer = setInterval(() => {
        time--
        this.time = time
        sessionStorage.setItem('time', time)
        this.showing = false
        if (time < 0) {
          this.clearTimer();
        }
      }, 1000);
    }
    this.resetForm();
    if (this.$store.state.userInfo && this.$store.state.userInfo.roleId === 1) {
      this.isUpdate = true;
      this.borrowerData.phone = this.$store.state.userInfo.phone;
      this.borrowerData.borrowerName = this.$store.state.userInfo.name;
    } else {
      this.isUpdate = false;
    }
    this.getSpecial();
    this.getProduct();
    this.getAgentData();
    this.getOrganData();
    this.getLoanData();
    this.getProvince();
  },
  methods: {
    clearTimer() {
      if (this.timer !== null || this.time < 0) {
        clearInterval(this.timer);
        this.showing = true;
        this.verifyCode = "重新获取";
        sessionStorage.setItem('time', 0)
      }
    },
    sendPhoneCode(phone) {
      // 发送验证码接口
      publicApi.sendPhoneCode(phone).then(res => {
        if (res.data.status === 200) {
          this.$message.success('验证码发送成功')
        } else {
          this.$message.warning(res.data.msg);
          this.clearTimer();
        }
      });
    },
    send() {
      //发送验证码按钮
      if (this.borrowerData.phone) {
        if (validaterPhone(this.borrowerData.phone)) {
          sessionStorage.setItem('time', 60)
          let time = 60
          this.timer = setInterval(() => {
            time--
            this.time = time
            sessionStorage.setItem('time', time)
            this.showing = false
            if (time < 0) {
              this.clearTimer();
            }
          }, 1000);
          if (this.$store.state.userInfo == null) { // 未登录状态
            publicApi.validateRegister(this.borrowerData.phone)
            .then(res2 => { // 验证该手机号是否已经注册
              if (res2.data.status === 500) {
                this.$message.warning("该手机号已被注册，请登录后在进行申请");
                this.clearTimer()
              } else {
                this.sendPhoneCode(this.borrowerData.phone);
              }
            });
          } else {
            if (this.$store.state.userInfo.roleId == 1) {
              validateApplyApi.public(this.borrowerData.phone).then(res => { // 验证该手机号今天是否在此申请过
                if (res.data.status == 200) { // 可以申请
                  this.sendPhoneCode(this.borrowerData.phone);
                } else {
                  this.$message.warning(res.data.msg);
                }
              });
            } else {
              this.$message.warning("贷款人方可申请");
              this.clearTimer()
            }
          }
        } else {
          this.$message.warning("手机号格式不正确");
        }
      } else {
        this.$message.warning("手机号不能为空");
      }
    },
    
    apply() {
      this.$refs.borrowerData.validate(valid => {
        if (valid) {
          if (this.isChecked) {
            let data = new FormData();
            for (let item in this.borrowerData) {
              data.append(item, this.borrowerData[item]);
            }
            this.$axios.post(`orderAll/saveNoLoginOrder`, data).then(res2 => {
              if (res2.status === 200) {
                this.clearTimer();
                if (this.$store.state.userInfo === null) {
                  this.$message.success("申请并注册成功");
                  this.$router.push({
                    path: "/applyVictory",
                    query: {
                      number: this.borrowerData.phone
                    }
                  });
                  this.$refs.borrowerData.resetFields()
                } else {
                  this.$message.success("申请成功");
                  this.$refs.borrowerData.resetFields()
                  this.resetForm()
                }
              } else {
                this.$message.warning(res2.msg);
              }
            })
          } else {
            this.$message.warning('请阅读并同意9能贷用户相关协议')
          }
        }
      });
    },
    //获取省
    getProvince() {
      this.$axios.get("city/getAllProvincial").then(res => {
        this.provinceData = res;
      });
    },
    //获取 市 区
    getCity(val) {
      this.borrowerData.address2 = ''
      this.$axios.get(`city/getAllCity/${val}`).then(res => {
        this.cityData = res;
      });
    },
    //特别推荐详情页
    specialLook(id) {
      this.$router.push(`/productDetail/${id}`);
    },
    proDetail(id) {
      this.$router.push(`/productDetail/${id}`);
    },
    orDetail(id) {
      this.$router.push(`/organDetail/${id}`);
    },
    agDetail(id) {
      this.$router.push(`/agentDetail/${id}`);
    },

    //特别推荐
    getSpecial() {
      this.$axios.get("product/getHotPro/1/4").then(res => {
        this.specialData = res;
      });
    },
    //产品推荐
    getProduct() {
      this.$axios.get("product/getLimitPro/1/3").then(res => {
        this.productData = res.list;
      });
    },
    //经纪人推荐
    getAgentData() {
      this.$axios.get("userBroker/showPageUserBroker").then(res => {
        this.agentData = res.rows;
      });
    },
    //机构推荐
    getOrganData() {
      this.$axios.get(`userAgency/indexAgency`).then(res => {
        this.organData = res;
      });
    },
    getLoanData() {
      this.$axios.get("userBorrower/indexBorrower").then(res => {
        this.loanData = res.map(item => {
          let fristName =
            item.borrowerName === null ? "无" : item.borrowerName.substr(0, 1);
          let allName =
            item.sex === 0 ? `${fristName}女士` : `${fristName}先生`;
          let phoneG =
            item.phone.substr(0, 3) + "*****" + item.phone.substr(8, 10);
          return { ...item, allName, phoneG };
        });
      });
    },
    handleBanner(val) {
      console.log(val)
      if (val === 0) {
        this.$router.push('/agent')
      } else if (val === 2) {
        this.$router.push('/organization')
      }
    },
    resetForm() {
      this.borrowerData = {
        borrowerName: "",
        sex: 1,
        loanAmount: "",
        address1: "",
        address2: "",
        phone: "",
        code: ""
      };
      if (this.$store.state.userInfo) {
        this.borrowerData.borrowerName = this.$store.state.userInfo.name
        this.borrowerData.phone = this.$store.state.userInfo.phone
      }
    }
  }
};
</script>
<style lang="scss">
.el-carousel, .el-carousel__container {
  height: 448px!important;
  min-width: 1200px;
}
.el-dropdown-menu__item {
  color: #4a90e2;
}
.apply {
  .el-checkbox__input.is-checked .el-checkbox__inner {
    background-color: #fff;
    border-color: #fff;
  }
  .el-checkbox__inner::after {
    border-color: #515151;
  }
  .el-dropdown {
    color: #fff;
  }
  .el-input {
    font-size: 16px;
  }
  .el-form-item {
    margin-bottom: 18px;
  }
}
</style>
<style scoped lang="scss">
$gray: #333;
.apply-btn {
  cursor: pointer;
}
.radio {
  margin-left: 10px;
  line-height: 35px;
}
/deep/ .el-radio__label {
  color: #fff;
}
/deep/ .is-checked .el-radio__inner {
  border-color: $gray;
  background: #fff;
  &:after {
    background-color: $gray;
  }
}
/deep/ .is-checked + .el-radio__label {
  color: #fff;
}
/deep/ .el-radio__inner:hover {
  border-color: $gray;
}
.organW {
  width: 100px;
  height: 139px;
  overflow: hidden;
  text-align: center;
  margin-right: 38px;
  margin-bottom: 24px;
}
.mb24 {
  margin-bottom: 24px;
}

.mt48 {
  margin-top: 48px;
}
.mr60 {
  margin-right: 60px;
}
.mt28 {
  margin-top: 28px;
}
.align {
  text-align: center;
}
.mr42 {
  margin-right: 39px;
}
.mr20 {
  margin-right: 10px;
}
.ml188 {
  margin-left: 188px;
}
.ml208 {
  margin-left: 208px;
}
.ml227 {
  margin-left: 227px;
}
.ml217 {
  margin-left: 217px;
}
.home {
  .img {
    position: relative;
    height:448px;
    img {
      width: 100%;
    height:448px;
    }
    .apply {
      position: absolute;
      left: 200px;
      top: 0;
      width: 376px;
      height: 100%;
      z-index: 998;
      background: rgba(0,0,0,.5);
      padding-top: 5px;
      box-sizing: border-box;
      font-family: PingFangSC-Regular;
      font-weight: 500;
      .title {
        text-align: center;
        font-size: 18px;
        font-weight: 400;
        color: #fff;
        margin-bottom: 10px;
      }
      .name-input {
        width: 120px;
      }
      .input-item {
        width: 256px;
      }
      .send {
        margin-left: 12px;
      }
      .agreement {
        color: #fff;
        text-align: center;
      }
    }
  }
}
.wrap {
  width: 100%;
  background: rgba(246, 246, 246, 1);
  padding-bottom: 30px;
  box-sizing: border-box;
  .common {
    margin-left: 24px;
    margin-top: 11px;
  }
}
.single {
  font-size: 0;
  .text1 {
    width: 54px;
    height: 25px;
    font-size: 18px;
    font-weight: 500;
    color: rgba(81, 81, 81, 1);
    line-height: 25px;
  }
  .text2 {
    height: 17px;
    font-size: 12px;
    font-weight: 400;
    color: rgba(155, 155, 155, 1);
    line-height: 17px;
  }
}
.special {
  width: 112px;
  height: 28px;
  font-size: 24px;
  font-weight: 500;
  color: rgba(0, 0, 0, 1);
  line-height: 28px;
}
.special-wrap {
  width: 1200px;
  overflow: hidden;
  .com {
    position: relative;
    text-align: center;
    overflow: hidden;
    width: 285px;
    height: 266px;
    background: rgba(255, 255, 255, 1);
    &:hover .btn {
      bottom: 10px;
    }
    &:hover {
      box-shadow: 0px 0px 13px 0px rgba(217, 217, 217, 1);
    }
    &:hover .word5 {
      transform: scale(0.1, 0.1);
    }
    .btn {
      position: absolute;
      bottom: -50px;
      width: 128px;
      height: 45px;
      cursor: pointer;
      background: rgba(168, 14, 14, 1);
      font-size: 14px;
      font-family: PingFangSC-Medium;
      font-weight: 500;
      color: rgba(255, 255, 255, 1);
      line-height: 45px;
      margin: 0 78px 0 78px;
      transition: bottom 0.2s ease;
    }
  }
  .word1 {
    height: 20px;
    font-size: 14px;
    font-weight: 400;
    color: rgba(155, 155, 155, 1);
    line-height: 20px;
    margin: 26px 89px 0 88px;
  }
  .word2 {
    width: 100px;
    height: 24px;
    background: rgba(168, 14, 14, 1);
    font-size: 14px;
    font-weight: 400;
    color: rgba(255, 255, 255, 1);
    line-height: 24px;
    margin-top: 8px;
    margin-left: auto;
    margin-right: auto;
  }
  .word3 {
    width: 85px;
    height: 53px;
    font-size: 38px;
    font-weight: bold;
    font-family: DINAlternate-Bold;
    color: rgba(168, 14, 14, 1);
    line-height: 53px;
    margin: 22px 100px 0 100px;
    .symbol {
      color: rgba(168, 14, 14, 1);
      font-size: 16px;
    }
  }
  .word4 {
    height: 13px;
    font-size: 13px;
    font-family: PingFangSC-Regular;
    font-weight: 400;
    color: rgba(155, 155, 155, 1);
    line-height: 13px;
    margin-bottom: 35px;
    margin-top: 5px;
  }
  .word5 {
    height: 24px;
    font-size: 17px;
    font-family: PingFangSC-Medium;
    font-weight: 500;
    color: rgba(155, 155, 155, 1);
    line-height: 24px;
    transition: transform 0.2s ease;
  }
}
.main {
  .more {
    height: 14px;
    font-size: 14px;
    font-family: PingFangSC-Regular;
    font-weight: 400;
    color: rgba(155, 155, 155, 1);
    line-height: 14px;
    margin-top: 14px;
    margin-left: 747px;
    cursor: pointer;
  }
  .img-right {
    margin-top: 14px;
  }
  .more-agent {
    height: 14px;
    font-size: 14px;
    font-family: PingFangSC-Regular;
    font-weight: 400;
    color: rgba(155, 155, 155, 1);
    line-height: 14px;
    margin-top: 14px;
    margin-left: 719px;
    cursor: pointer;
  }
  .product,
  .agent,
  .organize {
    height: 28px;
    font-size: 24px;
    font-family: PingFangSC-Medium;
    font-weight: 500;
    color: rgba(0, 0, 0, 1);
    line-height: 28px;
  }
  .product-wrap,
  .agent-wrap,
  .organize-wrap {
    width: 280px;
    height: 364px;
  }
  .product-wrap {
    background-image: url("../../static/resource/you.png");
    position: relative;
    .mark2 {
      position: absolute;
      left: 0;
      top: 0;
      bottom: 0;
      right: 0;
      background: rgba(0, 0, 0, .2);
    }
  }
  .agent-wrap {
    background-image: url("../../static/resource/agent.png");
    position: relative;
    .mark3 {
      position: absolute;
      left: 0;
      top: 0;
      bottom: 0;
      right: 0;
      background: rgba(0, 0, 0, .2);
    }
  }
  .organize-wrap {
    background-image: url("../../static/resource/organ.png");
    position: relative;
    .mark1 {
      position: absolute;
      left: 0;
      top: 0;
      bottom: 0;
      right: 0;
      background: rgba(0, 0, 0, .2);
    }
  }
  .agent-table,
  .organize-table {
    width: 590px;
    height: 364px;
    background: rgba(255, 255, 255, 1);
    padding: 24px 0 0 32px;
    box-sizing: border-box;
    margin-left: 30px;
  }
}
.product-table {
  width: 590px;
  height: 360px;
  background: rgba(255, 255, 255, 1);
  margin-left: 30px;
  .product-item {
    width: 590px;
    height: 121px;
    line-height: 121px;
    &:hover {
      box-shadow: 0px 4px 13px 0px rgba(236, 236, 236, 1);
    }

    .item1-num {
      height: 16px;
      font-size: 16px;
      font-family: PingFangSC-Regular;
      font-weight: 400;
      color: rgba(81, 81, 81, 1);
      line-height: 16px;
      margin-top: 34px;
      margin-left: 24px;
    }

    .item-same {
      height: 14px;
      font-size: 14px;
      font-family: PingFangSC-Regular;
      font-weight: 400;
      color: rgba(155, 155, 155, 1);
      line-height: 14px;
      margin-top: 27px;
    }
    .first {
      margin-left: 24px;
    }
    .item6-num {
      height: 16px;
      font-size: 16px;
      font-family: PingFangSC-Semibold;
      font-weight: 600;
      color: rgba(81, 81, 81, 1);
      line-height: 16px;
    }
    .item-num {
      height: 16px;
      font-size: 16px;
      font-family: DINAlternate-Bold;
      font-weight: bold;
      color: rgba(168, 14, 14, 1);
      line-height: 16px;
    }
    .percent {
      font-size: 12px;
      color: rgba(81, 81, 81, 1);
    }
    .item1 {
      width: 80px;
      margin-top: 33px;
      overflow: hidden;
    }
    .organ-btn {
      text-align: center;
      width: 80px;
      height: 50px;
      background: rgba(168, 14, 14, 1);
      font-size: 14px;
      font-family: PingFangSC-Medium;
      font-weight: 500;
      color: rgba(255, 255, 255, 1);
      line-height: 50px;
      margin-top: 36px;
      margin-bottom: 35px;
      margin-right: 20px;
      cursor: pointer;
    }
  }
}
.organ-text1 {
  text-align: center;
  width: 100px;
  overflow: hidden;
  height: 16px;
  font-size: 16px;
  font-family: PingFangSC-Semibold;
  font-weight: 600;
  color: rgba(81, 81, 81, 1);
  line-height: 16px;
  margin: 3px 0 6px 0;
}
.organ-text2 {
  text-align: center;
  width: 100px;
  overflow: hidden;
  height: 12px;
  font-size: 12px;
  font-family: PingFangSC-Regular;
  font-weight: 400;
  color: rgba(155, 155, 155, 1);
  line-height: 12px;
}
.loans {
  width: 268px;
  height: 532px;
  background: rgba(255, 255, 255, 1);
  margin-left: 20px;
  padding: 24px 35px 30px;
  box-sizing: border-box;
}
.agent-write {
  height: 30px;
  font-size: 24px;
  font-family: PingFangSC-Medium;
  font-weight: 500;
  color: rgba(255, 255, 255, 1);
  line-height: 30px;
  margin-top: 132px;
}
.agent-btn {
  width: 180px;
  height: 50px;
  background: rgba(239, 239, 239, 0.2);
  border: 1px solid rgba(255, 255, 255, 1);
  font-size: 18px;
  font-family: PingFangSC-Regular;
  font-weight: 400;
  color: rgba(255, 255, 255, 1);
  line-height: 50px;
  margin: 20px 50px;
  box-sizing: border-box;
  cursor: pointer;
}
.agent-name {
  width: 100px;
  overflow: hidden;
  height: 16px;
  font-size: 16px;
  font-family: PingFangSC-Semibold;
  font-weight: 600;
  color: rgba(81, 81, 81, 1);
  line-height: 16px;
  margin: 8px 0 6px 0;
}
.agent-job {
  width: 100px;
  overflow: hidden;
  height: 12px;
  font-size: 12px;
  font-family: PingFangSC-Regular;
  font-weight: 400;
  color: rgba(155, 155, 155, 1);
  line-height: 12px;
  margin-bottom: 8px;
}
.loans-use {
  height: 20px;
  font-size: 20px;
  font-family: PingFangSC-Medium;
  font-weight: 500;
  color: rgba(81, 81, 81, 1);
  line-height: 20px;
  margin-bottom: 26px;
}
.loans-text {
  margin: 5px 0 20px;
  height: 14px;
  font-size: 14px;
  font-family: PingFangSC-Regular;
  font-weight: 400;
  color: rgba(81, 81, 81, 1);
  line-height: 14px;
}
.message {
  width: 268px;
  height: 146px;
  background: rgba(255, 255, 255, 1);
  margin-top: 60px;
  margin-left: 20px;
  padding: 18px 18px;
  box-sizing: border-box;
  overflow: hidden;
  .mess-main {
    width: 180px;
    margin-left: 10px;
    box-sizing: border-box;
    overflow: hidden;
    .mess-text {
      width: 180px;
      height: 10px;
      font-size: 10px;
      font-family: PingFangSC-Regular;
      font-weight: 400;
      color: rgba(121, 183, 255, 1);
      line-height: 10px;
      margin-bottom: 10px;
    }
  }
}
// /deep/ .el-input__inner {
//   height: 30px;
//   line-height: 30px;
// }
// /deep/ .el-select__caret {
//   line-height: 30px;
// }
.empty-list {
  padding-top: 50px;
  text-align: center;
  color: #ccc;
  img {
    width: 200px;
    margin-bottom: 20px;
  }
}
</style>
