<template>
  <div>
    <div
      class="apply"
      v-loading="listLoading"
      element-loading-text="数据正在加载中..."
      element-loading-spinner="el-icon-loading"
      element-loading-background="rgba(0, 0, 0, 0.5)"
    >
      <div class="title">申请记录</div>
      <div class="header clearfix">
        <div class="fll text">名称</div>
        <div class="fll text">贷款金额</div>
        <div class="fll text">手机号</div>
        <div class="fll text">申请时间</div>
      </div>
      <div class="empty-list-show" v-show="!tableData.length">
        <img :src="emptyList" alt="">
        <p>暂无数据...</p>
      </div>
      <div
        class="table clearfix"
        v-for="(item,index) in tableData"
        :key="index"
        @click="enterUser(item)"
      >
        <div class="fll table-text">{{item.borrowerName == null ? '--' : item.borrowerName}}</div>
        <div class="fll table-text">{{item.loanAmount == null ? '--' : item.loanAmount}}</div>
        <div class="fll table-text">{{item.phone == null ? '--' : item.phone}}</div>
        <div class="fll table-text">{{item.createTime == null ? '--' : item.createTime}}</div>
      </div>
    </div>
    <div class="page" style="float: right;margin-top: 30px">
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
import emptyList from "../../../assets/empty-list2.png";
export default {
  name: "applyRecord",
  data() {
    return {
      emptyList,
      listLoading: true,
      tableData: [],
      page: 1,
      size: 9,
      count: 1
    };
  },
  methods: {
    enterUser(item) {
      this.$router.push(`/userDetail?type=2&orderId=${item.orderId}&borId=${item.borrowerId}`);
    },
    handleCurrentChange(val) {
      this.page = val;
      this.getData();
    },
    handleSizeChange(val) {
      this.size = val;
      this.getData();
    },
    getData() {
      this.listLoading = true;
      this.$axios
        .get(
          `orderAll/getOrderByAgency/${this.$store.state.userInfo.id}/${
            this.page
          }/${this.size}`
        )
        .then(res => {
          this.listLoading = false;
          this.tableData = res.data.list;
          this.count = res.data.totalCount;
          window.scrollTo(0, 0);
        });
    }
  },
  created() {
    this.getData();
  }
};
</script>

<style scoped lang="scss">
.apply {
  width: 944px;
  height: 696px;
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
      width: 208px;
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
    cursor: pointer;
    .table-text {
      width: 208px;
      line-height: 62px;
      font-size: 14px;
      font-family: PingFangSC-Regular;
      font-weight: 400;
      color: rgba(155, 155, 155, 1);
      text-align: center;
    }
  }
}
</style>
