<template>
  <div>
    <div class="apply">
      <div class="title">申请记录</div>
      <div
        class="wrap"
        v-loading="listLoading"
        element-loading-text="数据正在加载中..."
        element-loading-spinner="el-icon-loading"
        element-loading-background="rgba(0, 0, 0, 0.5)"
        style="min-height: 500px;"
      >
        <div class="header clearfix">
          <div class="fll text">名称</div>
          <div class="fll text">贷款金额</div>
          <div class="fll text">手机号</div>
          <div class="fll text">申请时间</div>
          <div class="fll text">操作</div>
        </div>
        
        <div class="empty-list-show" v-show="!tableData.length">
          <img :src="emptyList" alt="">
          <p>暂无数据...</p>
        </div>
        <div class="table clearfix" v-for="(item,index) in tableData" :key="index">
          <div class="fll table-text">{{item.name}}</div>
          <div class="fll table-text">{{item.amount}}</div>
          <div class="fll table-text">{{item.phone}}</div>
          <div class="fll table-text">{{item.time}}</div>
          <template v-if="item.orderState === 0 ? true : false">
            <div class="fll btn" @click="loanFail(item, 2)">
              <el-button size="mini" type="danger">贷款失败</el-button>
            </div>
            <div class="fll btn" @click="loanVictory(item, 1)">
              <el-button size="mini" type="success">贷款成功</el-button>
            </div>
          </template>
          <div class="fll table-text" v-if="item.orderState !== undefined && item.orderState !== 0">
            <el-tag
              :type="item.orderState | statusFilter"
              style="margin: 0 auto;"
            >{{ item.orderState | statesTextFilter }}</el-tag>
          </div>
        </div>
      </div>
    </div>
    <div class="page">
      <el-pagination
        @size-change="handleSizeChange"
        @current-change="handleCurrentChange"
        background=""
        :page-size="size"
        :pager-count="5"
        layout="prev, pager, next"
        :total="count"
      ></el-pagination>
    </div>
  </div>
</template>

<script>
import emptyList from '@/assets/empty-list2.png'
export default {
  filters: {
    statusFilter(status) {
      const statusMap = {
        1: "success",
        2: "danger"
      };
      return statusMap[status];
    },
    statesTextFilter(status) {
      const statusMap = {
        1: "成功",
        2: "失败"
      };
      return statusMap[status];
    }
  },
  data() {
    return {
      emptyList,
      listLoading: true,
      phoneId: "",
      loanId: "",
      isVictory: false,
      isMask: false,
      page: 1,
      size: 8,
      count: 1,
      tableData: [],
      loanData: [],
      xiuData: {
        orderId: "",
        phone: "",
        loanType: "",
        loanAmount: "",
        interest: ""
      }
    };
  },
  methods: {
    handleCurrentChange(val) {
      this.page = val;
      this.getData();
    },
    handleSizeChange(val) {
      this.size = val;
      this.getData();
    },
    loanFail(item, state) {
      // The loan failure
      this.$confirm("是否确认贷款失败？（此操作一旦完成不可回退）", "警告", {
        confirmButtonText: "确认",
        cancelButtonText: "取消",
        type: "warning"
      })
        .then(() => {
          const url = `/orderAll/updateState?orderId=${item.orderId}&roleId=${
            item.roleId
          }&ageBroId=${item.ageBroId}&state=${state}`;
          this.$axios.get(url).then(res => {
            if (res.status === 200) {
              this.$message.success("修改成功");
            } else {
              this.$message.fail(res.msg);
            }
            this.getData()
          });
        })
        .catch(() => {
          this.$message({
            type: "info",
            message: "已取消操作"
          });
        });
    },
    loanVictory(item, state) {
      // The loan success
      this.$confirm("是否确认贷款成功？（此操作一旦完成不可回退）", "警告", {
        confirmButtonText: "确认",
        cancelButtonText: "取消",
        type: "warning"
      })
        .then(() => {
          const url = `/orderAll/updateState?orderId=${item.orderId}&roleId=${
            item.roleId
          }&ageBroId=${item.ageBroId}&state=${state}`;
          this.$axios.get(url).then(res => {
            if (res.status === 200) {
              this.$message.success("修改成功");
            } else {
              this.$message.fail(res.msg);
            }
            this.getData()
          });
        })
        .catch(() => {
          this.$message({
            type: "info",
            message: "已取消操作"
          });
        });
    },
    getData() {
      this.listLoading = true;
      this.$axios
        .get(
          `orderAll/getOrderByBorrowerId/${this.$store.state.userInfo.id}/${
            this.page
          }/${this.size}`
        )
        .then(res => {
          if (res.status === 200) {
            this.listLoading = false;
            this.tableData = res.data.list;
            this.count = res.data.totalCount;
            window.scrollTo(0 , 0);
          }
        });
    }
  },
  created() {
    this.getData();
  }
};
</script>

<style scoped lang="scss">
.mt20 {
  margin-top: 15px;
}
.mask {
  position: fixed;
  left: 0;
  top: 0;
  bottom: 0;
  right: 0;
  background-color: #000;
  opacity: 0.5;
  z-index: 100;
}
.loan-vic {
  position: fixed;
  transform: translateX(-50%);
  left: 50%;
  top: 30%;
  z-index: 900;
}
.loan-vic .title {
  width: 400px;
  height: 40px;
  background: rgba(208, 172, 86, 1);
  font-size: 16px;
  font-family: PingFangSC-Regular;
  font-weight: 400;
  text-align: center;
  color: rgba(255, 255, 255, 1);
  line-height: 40px;
}
.loan-vic .loan-con {
  padding: 20px 70px;
  box-sizing: border-box;
  width: 400px;
  height: 300px;
  background: rgba(255, 255, 255, 1);
  .text {
    width: 70px;
    font-size: 14px;
    font-family: PingFangSC-Regular;
    font-weight: 400;
    color: rgba(155, 155, 155, 1);
    line-height: 40px;
  }
  input {
    height: 24px;
    line-height: 40px;
    margin-top: 6px;
  }
  button {
    cursor: pointer;
    width: 60px;
    height: 30px;
    background: rgba(208, 172, 86, 1);
    text-align: center;
    font-size: 14px;
    font-family: PingFangSC-Regular;
    font-weight: 400;
    color: rgba(255, 255, 255, 1);
    line-height: 30px;
    margin-top: 30px;
    margin-left: 100px;
  }
}

.page {
  float: right;
  margin-top: 30px;
}
.apply {
  width: 944px;
  height: 600px;
  background: rgba(255, 255, 255, 1);
  padding: 24px 56px;
  box-sizing: border-box;
  .title {
    height: 42px;
    font-size: 16px;
    font-family: PingFangSC-Medium;
    font-weight: 500;
    color: rgba(81, 81, 81, 1);
    line-height: 16px;
  }
  .header {
    width: 832px;
    height: 40px;
    background: rgba(250, 250, 250, 1);
    .text {
      width: 166px;
      text-align: center;
      font-size: 14px;
      font-family: PingFangSC-Regular;
      font-weight: 400;
      color: rgba(155, 155, 155, 1);
      line-height: 40px;
    }
  }
  .table {
    width: 832px;
    height: 62px;
    border-bottom: 1px solid #f0f0f0;
    .table-text {
      width: 166px;
      line-height: 62px;
      font-size: 14px;
      font-family: PingFangSC-Regular;
      font-weight: 400;
      color: rgba(155, 155, 155, 1);
      text-align: center;
    }
    .btn {
      width: 60px;
      height: 30px;
      text-align: center;
      border-radius: 4px;
      font-size: 14px;
      font-family: PingFangSC-Regular;
      font-weight: 400;
      color: rgba(255, 255, 255, 1);
      line-height: 30px;
      margin-top: 16px;
      margin-left: 15px;
      cursor: pointer;
    }
  }
}
</style>
