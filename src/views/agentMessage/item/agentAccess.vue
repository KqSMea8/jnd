<template>
  <div>
    <div class="title">访问记录</div>
    <div
      class="wrap"
      v-loading="listLoading"
      element-loading-text="数据正在加载中..."
      element-loading-spinner="el-icon-loading"
      element-loading-background="rgba(0, 0, 0, 0.5)"
    >
      <div class="empty-list-show" v-show="!tableData.length">
        <img :src="emptyList" alt="">
        <p>暂无数据...</p>
      </div>
      <!-- .borrower.borrowerId -->
      <div class="record-item" v-for="(item,index) in tableData" :key="index" @click="pushUserDetail(item)"> 
        <div class="year fll">{{item.startTime}}</div>
        <div class="year fll">{{item.borrower.borrowerName}}</div>
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
import emptyList from "../../../assets/empty-list2.png";
export default {
  name: "agentAccess",
  data() {
    return {
      emptyList,
      listLoading: true,
      tableData: [],
      page: 1,
      size: 10,
      count: 1
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
    getData() {
      this.listLoading = true;
      this.$axios
        .get(
          `brokerBrowsedRecord/showPageRecordByBrokerId/${
            this.$store.state.userInfo.id
          }?pn=${this.page}&page=${this.size}`
        )
        .then(res => {
          this.tableData = res.rows;
          this.count = res.total;
          this.listLoading = false;
          window.scrollTo(0, 0);
        });
    },
    pushUserDetail(item) {
      this.$router.push(`/userDetail?type=3&roleId=${this.$store.state.userInfo.roleId}&brokerOrAgencyId=${this.$store.state.userInfo.id}&borId=${item.borrower.borrowerId}`);
    }
  },
  created() {
    this.getData();
  }
};
</script>

<style scoped lang="scss">
.title {
  width: 888px;
  height: 66px;
  padding-left: 56px;
  font-size: 16px;
  font-family: PingFangSC-Medium;
  font-weight: 500;
  color: rgba(81, 81, 81, 1);
  line-height: 66px;
  background: rgba(255, 255, 255, 1);
  box-sizing: border-box;
}
.wrap {
  min-height: 500px;
}
.record-item {
  background: rgba(255, 255, 255, 1);
  box-sizing: border-box;
  height: 70px;
  line-height: 70px;
  border-bottom: 1px solid #f0f0f0;
  cursor: pointer;
  .year {
    margin-left: 56px;
    font-size: 14px;
    font-family: PingFangSC-Regular;
    font-weight: 400;
    color: rgba(81, 81, 81, 1);
    line-height: 70px;
  }
}

.page {
  text-align: right;
  margin-top: 25px;
}
</style>


