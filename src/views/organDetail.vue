<template>
  <div>
    <div class="wrappy">
      <div class="w1200">
        <div class="person clearfix mb30">
          <img
            :src="organDetail.agencyLogo"
            v-if="organDetail.agencyLogo"
            alt=""
            class="fll"
            style="width: 120px;height: 120px"
          >
          <img
            src="/static/resource/pic/organ.png"
            v-else
            alt=""
            class="fll"
            style="width: 120px;height: 120px"
          >
          <div class="fll">
            <div class="clearfix">
              <div class="fll" style="margin-left: 14px">
                <div class="clearfix">
                  <div class="name fll">{{organDetail.agencyName}}</div>
                  <img src="../../static/resource/agent/vip.png" alt="">
                </div>
                <a :href="'http://' + organDetail.agencyURL" target="_blank">{{organDetail.agencyURL}}</a>
              </div>
              <div class="agent-detail clearfix fll">
                <div class="fll">
                  <div class="clearfix">
                    <div class="fll align">
                      <div class="num">{{organDetail.agencyAgeLimit}}</div>
                      <div class="clearfix mt16">
                        <img src="../../static/resource/agent/1.png" alt="" class="fll">
                        <div class="num-text fll">成立年限</div>
                      </div>
                    </div>
                    <div class="fll align ml20">
                      <div class="num">{{organDetail.agencyTotalService}}</div>
                      <div class="clearfix mt16">
                        <img src="../../static/resource/agent/1.png" alt="" class="fll">
                        <div class="num-text fll">服务客户</div>
                      </div>
                    </div>
                    <div class="fll align ml20">
                      <div class="num">{{organDetail.agencyLoansAmount}}</div>
                      <div class="clearfix mt16">
                        <img src="../../static/resource/agent/1.png" alt="" class="fll">
                        <div class="num-text fll">批贷总额</div>
                      </div>
                    </div>
                  </div>
                </div>
                <div style="margin-left: 20px" class="clearfix fll">
                  <img
                    src="../../static/resource/agent/tel.png"
                    alt=""
                    class="fll"
                    style="width: 56px;height: 47px"
                  >
                  <div class="fll">
                    <div class="text1">电话咨询贷款，最快捷，最方便！</div>
                    <div class="text2">{{organDetail.phone}}</div>
                  </div>
                </div>
              </div>
            </div>
            <div class="clearfix ml20">
              <div class="clearfix fll" style="margin-top: 16px;">
                <img
                  src="../assets/message.png"
                  alt=""
                  class="fll"
                  style="width: 20px;height: 20px;vertical-align: middle; margin-right: 10px;"
                >
                <div class="restore fll" @click="sendMess">发送消息</div>
              </div>
              <div class="clearfix flr" style="margin-top: 16px;">
                <img
                  src="../../static/resource/agent/star.png"
                  alt=""
                  class="fll"
                  style="width: 20px;height: 20px;vertical-align: middle; margin-right: 10px;"
                >
                <div class="fll">
                  <div class="restore" @click="restore" v-if="isCollect">加入收藏</div>
                  <div class="restore" @click="cancelRestore" v-else>取消收藏</div>
                </div>
              </div>
            </div>
          </div>
        </div>
        <div class="clearfix">
          <div class="fll">
            <div class="top">
              <div class="title">9能贷网诚信认证</div>
              <div class="content-agent">
                <div class="name">{{organDetail.agencyName}}</div>
                <div class="name">实名认证：{{organDetail.nameAuthentication}}</div>
                <div class="name">营业执照：{{organDetail.licenseAuthentication}}</div>
                <div class="name">营业执照：{{organDetail.licenseAuthentication}}</div>
                <div class="clearfix name-same" style="padding: 10px 0; border-bottom: 1px solid #f0f0f0;">
                  <div
                    class="name fll"
                    style="height: 18px;line-height: 18px; border: none;"
                  >推荐指数：</div>
                  <div class="fll">
                    <el-rate v-model="organDetail.agencyStar" disabled text-color="#ff9900"></el-rate>
                  </div>
                </div>
                
                <div class="name">所属公司：{{organDetail.companyOwned}}</div>
                <div class="desc-title">机构简介：</div>
                <div class="desc">{{organDetail.agencyIntroduction}}</div>
              </div>
            </div>
            <div class="bottom" v-if="loanData.length > 0">
              <div class="title">成功案例</div>
              <div class="content-item" v-for="(item,index) in loanData" :key="index">
                <span class="name">{{item.borName}}</span><span class="money">贷款金额：{{item.proAmount}}万</span>
              </div>
            </div>
          </div>
          <div class="fll">
            
            <div class="bottom-form">
              <div class="clearfix main-form" v-for="(item,index) in productData" :key="index">
                <div class="fll" style="width: 180px;overflow: hidden">
                  <div class="item1-num">{{item.productName}}</div>
                  <div class="item-same">无需抵车即可贷款</div>
                </div>
                <div class="item1 fll align">
                  <div class="item-num">{{item.productInterest}}%</div>
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
                  <div class="item-num">{{item.productStartAmount}}万起</div>
                  <div class="item-same">起贷金额</div>
                </div>
                <div class="flr organ-btn align" @click="lookDetail(item.productId)">立即查看</div>
              </div>
            </div>
            <div class="top-form">
              <el-form ref="agentApplyForm" :model="borrowerData" :rules="borrowerDataRules" label-position="right" label-width="96px">
                <el-form-item label="姓名：" prop="borrowerName">
                  <el-input :disabled="!!this.$store.state.userInfo" v-model="borrowerData.borrowerName" class="input-item"></el-input>
                  <el-radio-group v-model="borrowerData.sex" :disabled="!!this.$store.state.userInfo" style="margin-left: 10px;">
                    <el-radio :label="1">男</el-radio>
                    <el-radio :label="0">女</el-radio>
                  </el-radio-group>
                </el-form-item>
                <el-form-item label="贷款金额：" prop="loanAmount">
                  <el-input v-model="borrowerData.loanAmount" class="input-item"></el-input>
                </el-form-item>
                <el-form-item label="所在地：" prop="address">
                  <el-select v-model="borrowerData.address1" style="width: 100px;" clearable @change="getCity">
                    <el-option
                      v-for="item in provinceData" :key="item.pid" :label="item.provincial" :value="item.pid"></el-option>
                  </el-select>
                  <el-select v-model="borrowerData.address2" clearable style="margin-left: 10px;width: 100px;">
                    <el-option v-for="item in cityData" :key="item.cid" :label="item.city" :value="item.cid"></el-option>
                  </el-select>
                </el-form-item>
                <el-form-item label="业务类型：" prop="businessType">
                  <el-radio-group v-model="borrowerData.businessType">
                    <el-radio :label="1">个人贷款</el-radio>
                    <el-radio :label="2">企业贷款</el-radio>
                  </el-radio-group>
                </el-form-item>
                <el-form-item label="有无抵押：" prop="isPawn">
                  <el-radio-group v-model="borrowerData.isPawn">
                    <el-radio :label="1">有抵押</el-radio>
                    <el-radio :label="0">无抵押</el-radio>
                  </el-radio-group>
                </el-form-item>
                <el-form-item v-if="borrowerData.isPawn !== 0" label="抵押物：" prop="pawnKey">
                  <el-checkbox-group v-model="borrowerData.pawnKey">
                    <el-checkbox
                      :label="item.pawnId"
                      :key="index"
                      v-for="(item,index) in pawnData"
                    >{{item.pawn}}</el-checkbox>
                  </el-checkbox-group>
                </el-form-item>
                <el-form-item v-else label="无抵押：" prop="noPawn">
                  <el-select v-model="borrowerData.age" style="width: 100px;" placeholder="年龄">
                    <el-option v-for="item in ageData" :key="item.id" :label="item.ageArea" :value="item.id"></el-option>
                  </el-select>

                  <el-select v-model="borrowerData.borrowerJob" style="margin-left: 5px;width: 100px;" placeholder="职业">
                    <el-option v-for="item in jobData" :key="item.jobId" :label="item.jobName" :value="item.jobId" style="width: 100px;"></el-option>
                  </el-select>

                  <el-select v-model="borrowerData.borrowerMonthlyIncome" style="margin-left: 5px;width: 100px;" placeholder="月收入">
                    <el-option v-for="item in monthMoneyData" :key="item.id" :label="item.incomeName" :value="item.id"></el-option>
                  </el-select>
                </el-form-item>

                <el-form-item v-if="!userInfo" label="手机号：" prop="phone">
                  <el-input v-model="borrowerData.phone" class="input-item"></el-input>
                </el-form-item>
                <el-form-item v-if="!userInfo" label="验证码：" prop="code">
                  <el-input v-model="borrowerData.code" style="width: 90px;"></el-input>
                  <el-button v-if="showing" @click="send" style="width: 110px;">{{ verifyCode }}</el-button>
                  <el-button v-else @click="send" style="width: 110px;">{{ time }}s</el-button>
                </el-form-item>
                <el-form-item prop="agree">
                  <el-checkbox style="display: inline-block" v-model="agree">阅读并同意</el-checkbox><a href="#/agreement?userRegister" target="_blank" style="color: #4a90e2;">《9能贷用户注册协议》</a><a href="#/agreement?userProtect" target="_blank" style="color: #4a90e2;">《用户隐私保护政策》</a>
                </el-form-item>
                <el-form-item>
                  <el-button class="apply" @click="freeApply">免费申请</el-button>
                </el-form-item>
              </el-form>
            </div>
          </div>
        </div>
        <footerSame></footerSame>
      </div>
    </div>
  </div>
</template>

<script>
import { mapState } from "vuex";
import footerSame from "../component/footerSame";
import { validaterPhone, validaterName, validaterLoanAmount } from "@/util/validate";
export default {
  name: "organDetail",
  components: {
    footerSame
  },
  data() {
    
    const validatePhone = (rule, value, callback) => {
      if (!value) {
        callback(new Error("手机号不能为空"));
      } else {
        if (validaterPhone(value)) {
          callback();
        } else {
          callback(new Error("手机号格式不正确"));
        }
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
    const validateAddress = (rule, value, callback) => {
      if (this.borrowerData.address1 && this.borrowerData.address2) {
        callback()
      } else {
        callback(new Error('请选择地址'))
      }
    }
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
    const validatePawnKey = (rule, value, callback) => {
      if (this.borrowerData.isPawn) {
        if (this.borrowerData.pawnKey.length === 0) {
          callback(new Error('请选择抵押物'))
        } else {
          callback()
        }
      } else {
        callback()
      }
    }
    const validateNoPawn  = (rule, value, callback) => {
      if (!this.borrowerData.isPawn) {
        if (this.borrowerData.age !== '' && this.borrowerData.borrowerJob !== '' && this.borrowerData.borrowerMonthlyIncome !== '') {
          callback()
        } else {
          callback(new Error('请将相关信息选择完整'))
        }
      } else {
        callback()
      }
    }
    const validateAgree = (rule, value, callback) => {
      if (this.agree) {
        callback()
      } else {
        callback(new Error('请阅读并同意9能贷相关协议'))
      }
    }
    return {
      Agree: true,
      isCollect: true,
      businessArea1: "",
      businessArea2: "",
      showing: true,
      time: 60,
      organId: "",
      verifyCode: "发送验证码",
      radio1: "1",
      radio2: "1",
      organDetail: {
        agencyId: "",
        // 机构名称
        agencyName: "",
        // 机构属性
        agencyProperty: "",
        // 机构代码
        agencyCode: "",
        // 机构介绍
        agencyIntroduction: "",
        // 营业执照
        agencyLicense: "",
        // 成立年限
        agencyAgeLimit: "",
        // 营业期限
        startBusiness: "",
        endBusiness: "",
        // 机构网址
        agencyURL: "",
        // 所属公司
        companyOwned: "",
        // 机构所在地
        agencyAddress: "",
        // 填写人姓名
        agencyUsername: "",
        // 填写人身份
        agencyIdentity: "",
        // 填写人手机号
        phone: "",
        // 填写人邮箱
        email: "",
        // 邮箱是否认证
        emailAuthentication: "",
        // 机构logo
        agencyLogo: "",
        // 是否是会员
        member: "",
        // 会员期限
        memberTime: "",
        // 服务客户
        agencyTotalService: "",
        // 批贷总额
        agencyLoansAmount: "",
        // 实名认证
        nameAuthentication: "",
        // 营业执照认证
        licenseAuthentication: "",
        // 推荐指数
        agencyStar: 0,
        // 注册时间
        agencyTime: "",
        // 被浏览次数
        agency1: ""
      },
      
      borrowerDataRules: {
        borrowerName: [{ required: true, trigger: "change", validator: validateName }],
        sex: [{ required: true, trigger: "change", message: '请选择性别' }],
        loanAmount: [{ required: true, trigger: "change", validator: validateLoanAmount }],
        address: [{ required: true, trigger: "change", validator: validateAddress }],
        businessType: [{ required: true, trigger: "change", message: '请选择所在地' }],
        isPawn: [{ required: true, trigger: "change", message: '请选择有无抵押' }],
        pawnKey: [{ required: true, trigger: "change", validator: validatePawnKey }],
        noPawn: [{ required: true, trigger: "change", validator: validateNoPawn }],
        phone: [{ required: true, trigger: "change", validator: validatePhone }],
        code: [{ required: true, trigger: "change", message: '验证码不能为空' }],
        agree: [{ required: true, trigger: "change", validator: validateAgree }]
      },
      loanData: [],
      productData: [],
      //省
      provinceData: [],
      //市 区
      cityData: [],
      agree: true,
      //申请贷款
      borrowerData: {
        agencyId: this.organId,
        borrowerName: "",
        sex: 1,
        loanAmount: "",
        address1: "",
        address2: "",
        businessType: 1, // 贷款类型
        isPawn: 0, // 有无抵押
        phone: "",
        code: "",
        pawnKey: [], // 抵押物
        age: "",
        borrowerJob: "",
        borrowerMonthlyIncome: ""
      },
      pawnData: [],
      monthMoneyData: [],
      jobData: [],
      ageData: [],
      timer: null
    };
  },
  computed: {
    ...mapState(["userInfo"])
  },
  watch: {
    userInfo(val) {
      this.borrowerData.borrowerName = val.name
      this.borrowerData.phone = val.phone
    }
  },
  methods: {
    //发送消息
    sendMess() {
      if (this.$store.state.userInfo) {
        if (this.$store.state.userInfo.roleId === 1) {
          this.$router.push({
            path: `/myMessage/${this.$store.state.userInfo.id}/messageCenter`,
            query: { id: this.organId, roleId: 3 }
          });
        } else {
          this.$message.warning("借款人方可发送信息");
        }
      } else {
        this.$message.warning("请先登录");
      }
    },
    lookDetail(id) {
      this.$router.push(`/productDetail/${id}`);
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
    clearTimer() {
      clearInterval(this.timer);
      this.showing = true;
      this.verifyCode = "重新获取";
      this.time = 60;
    },
    //发送验证码
    send() {
      if (this.borrowerData.phone) {
        let value = this.borrowerData.phone;
        if (!/^[1][34578]\d{9}$/.test(value) || !/^[1-9]\d*$/.test(value) || value.length !== 11) {
          this.$message.warning("手机号不符合规范");
          this.borrowerData.phone = "";
        } else {
          this.showing = false;
          this.timer = setInterval(() => {
            this.time--;
            if (this.time < 0) {
              this.clearTimer()
            }
          }, 1000);
          this.$axios
            .get(`user/selectPhone/${this.borrowerData.phone}`)
            .then(res => {
              if (res.status === 200) {
                this.$message.success("您已注册该平台，请登录");
                this.resetForm()
                this.clearTimer()
              } else {
                this.$axios.get(`base/getUpdatePhoneCode/${this.borrowerData.phone}`).then(res => {});
              }
            });
        }
      } else {
        this.$message.warning("手机号不能为空");
      }
    },
    //机构详情
    getDetail(id) {
      this.$axios.get(`userAgency/selectUserAgencyById/${id}`).then(res => {
        this.organDetail = res;
      });
    },

    //机构代理的产品
    getProduct(id) {
      this.$axios.get(`product/selectProductByAgency/${id}/1/4`).then(res => {
        this.productData = res.list;
      });
    },
    //机构的成功案例
    getVictory(id) {
      this.$axios.get(`orderAll/getAgencySuccessOrder/${id}`).then(res => {
        this.loanData = res;
      });
    },
    //收藏机构
    restore() {
      if (this.$store.state.userInfo) {
        if (this.$store.state.userInfo.roleId === 1) {
          this.$axios
            .post(
              `borAgency/addBorAgency?borId=${
                this.$store.state.userInfo.id
              }&agencyId=${this.organId}`
            )
            .then(res => {
              this.isCollect = false;
              this.$message.success("收藏成功");
            });
        } else {
          this.$message.warning("借款人方可收藏");
        }
      } else {
        this.$message.warning("请先登录");
      }
    },
    //取消机构
    cancelRestore() {
      if (this.$store.state.userInfo) {
        this.$axios
          .post(
            `borAgency/deleteBorAgency?borId=${
              this.$store.state.userInfo.id
            }&agencyId=${this.organId}`
          )
          .then(res => {
            this.isCollect = true;
            this.$message.success("取消收藏");
          });
      }
    },

    //免费申请
    freeApply() {
      this.$refs.agentApplyForm.validate(valid => {
        console.log(valid)
        if (valid) {
          let data = new FormData();
          this.borrowerData.agencyId = this.organId
          for (let item in this.borrowerData) {
            data.append(item, this.borrowerData[item]);
          }

          if (this.$store.state.userInfo) {
            if (this.$store.state.userInfo.roleId === 1) {
              data.delete("phone");
              data.delete("code");
              data.append("borrowerId", this.$store.state.userInfo.id);
              this.$axios.post("orderAll/saveLoginOrder", data).then(res => {
                if (res.status === 200) {
                  this.$message.success(res.msg);
                } else {
                  this.$message.warning(res.msg);
                }
                if (this.timer !== null) {
                  this.clearTimer();
                }
              });
              this.resetForm();
            } else {
              this.$message.warning("借款人方可申请");
            }
          } else {
            this.$axios.post("orderAll/saveNoLoginOrder", data).then(res => {
              if (res.status === 200) {
                this.$message.success(res.msg);
                this.$router.push({
                  path: "/applyVictory",
                  query: {
                    number: this.borrowerData.phone
                  }
                });
              } else {
                this.$message.warning(res.msg);
              }
              setTimeout(() => {
                this.resetForm();
              }, 100);
              
              if (this.timer !== null) {
                this.clearTimer();
              }
            });
          }
        }
      })
    },

    //借款人浏览机构
    skimRecord() {
      if (this.$store.state.userInfo) {
        if (this.$store.state.userInfo.roleId === 1) {
          let data = new FormData();
          data.append("borId", this.$store.state.userInfo.id);
          data.append("agencyId", this.organId);
          this.$axios.post(`borLook/addLook`, data).then(res => {
            if (res.status === 200) {
            }
          });
        }
      }
    },

    collectPan() {
      if (this.$store.state.userInfo) {
        let data = new FormData();
        data.append("borId", this.$store.state.userInfo.id);
        data.append("agencyId", this.organId);
        this.$axios.post(`borAgency/selectBorAgency`, data).then(res => {
          if (res === 0) {
            this.isCollect = true;
          } else {
            this.isCollect = false;
          }
        });
      }
    },
    //获得所有抵押物信息
    getPawn() {
      this.$axios.get("/pawn/getAllPawn").then(res => {
        this.pawnData = res.filter(item => {
          return item.pawnId !== 1;
        });
      });
    },
    getAge() {
      this.$axios.get(`age/getAllAgeArea`).then(res => {
        this.ageData = res;
      });
    },
    getMonthMoney() {
      this.$axios.get("get/getIncome").then(res => {
        this.monthMoneyData = res;
      });
    },
    getJob() {
      this.$axios.get("get/getJob").then(res => {
        res.forEach(item => {
          if (item.jobId === 0) {
            item.jobName = "其他";
          }
        });
        this.jobData = res;
      });
    },
    resetForm() {
      this.borrowerData = {
        brokerId: "",
        borrowerName: this.$store.state.userInfo.name,
        sex: 1,
        loanAmount: "",
        address1: "",
        address2: "",
        businessType: 1,
        isPawn: 0,
        phone: "",
        code: "",
        pawnKey: [],
        age: "",
        borrowerJob: "",
        borrowerMonthlyIncome: ""
      }
    }
  },
  created() {
    if (this.$store.state.userInfo) {
      this.borrowerData.borrowerName = this.$store.state.userInfo.name
      this.borrowerData.sex = this.$store.state.userInfo.sex
    }
    this.organId = this.$route.params.id;
    this.getDetail(this.organId);
    this.getProduct(this.organId);
    this.getVictory(this.organId);
    this.getProvince();
    this.skimRecord();
    this.getPawn();
    //年龄
    this.getAge();
    //职业
    this.getJob();
    //月收入
    this.getMonthMoney();
    //收藏判断
    this.collectPan();
  }
};
</script>

<style scoped lang="scss">

.top-form {
  .input-item {
    width: 210px;
  }
  // .el-form-item {
  //   margin-bottom: 16px;
  // }
  
  .el-checkbox__input.is-checked + .el-checkbox__label {
    color: rgba(81, 81, 81, 1);
  }
  .el-checkbox__inner {
    background-color: #fff;
    border-color: #ccc;
  }
  .el-checkbox__input.is-checked + .el-checkbox__inner {
    background-color: rgba(208, 172, 86, 1);
    border-color: rgba(208, 172, 86, 1);
  }
  .apply {
    background: #A80E0E;
    color: #fff;
    border-color: #A80E0E;
  }
}
.cheak {
  margin-top: 3px;
  margin-left: 100px;
}

.read {
  display: inline-block;
  height: 17px;
  font-size: 12px;
  font-family: PingFangSC-Regular;
  font-weight: 400;
  color: #7b7b7b;
  line-height: 17px;
  .blue {
    color: #4a90e2;
  }
}

.mt16 {
  margin-top: 16px;
}

.ml20 {
  margin-left: 20px;
}

.mb30 {
  margin-bottom: 30px;
}

.person {
  width: 1200px;
  height: 160px;
  background: rgba(255, 255, 255, 1);
  padding: 20px 60px;
  box-sizing: border-box;
  .name {
    height: 28px;
    font-size: 20px;
    font-family: PingFangSC-Medium;
    font-weight: 500;
    color: rgba(81, 81, 81, 1);
    line-height: 28px;
  }
  a {
    display: block;
    width: 173px;
    height: 35px;
    font-size: 14px;
    font-family: PingFangSC-Regular;
    font-weight: 400;
    color: rgba(155, 155, 155, 1);
    line-height: 20px;
    overflow: hidden;
    margin-top: 8px;
  }
  .restore {
    line-height: 20px;
    vertical-align: middle;
    font-size: 16px;
    font-family: PingFangSC-Regular;
    font-weight: 400;
    color: rgba(155, 155, 155, 1);
    cursor: pointer;
  }
  .agent-detail {
    height: 50px;
    margin-top: 20px;
    margin-left: 100px;
    .num {
      height: 20px;
      font-size: 20px;
      font-family: PingFangSC-Medium;
      font-weight: 500;
      color: rgba(81, 81, 81, 1);
      line-height: 20px;
      letter-spacing: 2px;
    }
    .num-text {
      height: 12px;
      font-size: 12px;
      font-family: PingFangSC-Regular;
      font-weight: 400;
      color: rgba(155, 155, 155, 1);
      line-height: 12px;
    }
    .text1 {
      height: 20px;
      font-size: 18px;
      font-family: PingFangSC-Medium;
      font-weight: 500;
      color: rgba(81, 81, 81, 1);
      line-height: 20px;
      margin-left: 10px;
    }
    .text2 {
      height: 24px;
      font-size: 20px;
      font-family: DINAlternate-Bold;
      font-weight: bold;
      color: rgba(168, 14, 14, 1);
      line-height: 24px;
      letter-spacing: 4px;
      margin-top: 10px;
      margin-left: 10px;
    }
  }
}

.top {
  width: 268px;
  .title {
    text-align: center;
    height: 60px;
    background: rgba(208, 172, 86, 1);
    font-size: 20px;
    font-family: PingFangSC-Medium;
    font-weight: 500;
    color: rgba(255, 255, 255, 1);
    line-height: 60px;
  }
  .content-agent {
    background: rgba(255, 255, 255, 1);
    padding: 20px 10px;
    box-sizing: border-box;
    .name {
      border-bottom: 1px solid #f0f0f0;
      padding: 10px 0;
      font-size: 14px;
      font-family: PingFangSC-Regular;
      font-weight: 400;
      color: rgba(81, 81, 81, 1);
    }
    .desc,
    .desc-title {
      font-size: 14px;
      font-family: PingFangSC-Regular;
      font-weight: 400;
      color: rgba(81, 81, 81, 1);
    }
    .desc-title {
      margin-top: 10px;
      margin-bottom: 8px;
      line-height: 14px;
      height: 14px;
    }
    .desc {
      width: 210px;
      height: 80px;
      line-height: 20px;
      overflow: hidden;
    }
    .company-main {
      height: 55px;
      border-bottom: 1px solid #f0f0f0;
      .com-same {
        font-size: 14px;
        font-family: PingFangSC-Regular;
        font-weight: 400;
        color: rgba(81, 81, 81, 1);
        line-height: 20px;
      }
    }
  }
}

.bottom {
  width: 268px;
  margin-top: 30px;
  .title {
    height: 60px;
    background: rgba(208, 172, 86, 1);
    text-align: center;
    font-size: 20px;
    font-family: PingFangSC-Medium;
    font-weight: 500;
    color: rgba(255, 255, 255, 1);
    line-height: 60px;
  }
  .content-item {
    padding: 20px;
    box-sizing: border-box;
    border-bottom: 1px solid #f0f0f0;
    background-color: #fff;
    font-size: 14px;
    color: #515151;
    overflow: hidden;
    .name {
      float: left;
      width: 50%;
    }
    .money {
      float: right;
      width: 50%;
    }
  }
}

.top-form {
  width: 902px;
  background: rgba(255, 255, 255, 1);
  margin-left: 30px;
  padding: 24px 56px 30px 56px;
  box-sizing: border-box;
}

.bottom-form {
  width: 902px;
  background: rgba(255, 255, 255, 1);
  margin-bottom: 30px;
  margin-left: 30px;
  .main-form {
    height: 87px;
    padding: 20px 20px 20px 30px;
    box-sizing: border-box;
    border-bottom: 1px solid #f0f0f0;
    .item1-num {
      height: 16px;
      font-size: 16px;
      font-family: PingFangSC-Regular;
      font-weight: 400;
      color: rgba(81, 81, 81, 1);
      line-height: 16px;
    }
    .item-same {
      height: 14px;
      font-size: 14px;
      font-family: PingFangSC-Regular;
      font-weight: 400;
      color: rgba(155, 155, 155, 1);
      line-height: 14px;
      margin-top: 16px;
    }
    .item-num {
      height: 16px;
      font-size: 16px;
      font-family: DINAlternate-Bold;
      font-weight: bold;
      color: rgba(81, 81, 81, 1);
      line-height: 16px;
    }
    .item1 {
      width: 128px;
      text-align: center;
      overflow: hidden;
    }
    .organ-btn {
      width: 96px;
      height: 48px;
      background: rgba(168, 14, 14, 1);
      font-size: 16px;
      font-family: PingFangSC-Medium;
      font-weight: 500;
      color: rgba(255, 255, 255, 1);
      line-height: 48px;
      margin-left: 60px;
      cursor: pointer;
    }
  }
}
</style>
