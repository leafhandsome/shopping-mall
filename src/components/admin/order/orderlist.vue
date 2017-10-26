<template>
  <div class="arttmpl">
    <el-row>
      <el-col>
        <div class="abread bt-line">
          <!-- //面包屑 -->
          <el-breadcrumb separator="/">
            <el-breadcrumb-item :to="{ path: '/' }">首页 </el-breadcrumb-item>
            <el-breadcrumb-item>订单管理</el-breadcrumb-item>
            <el-breadcrumb-item>订单列表</el-breadcrumb-item>

          </el-breadcrumb>
        </div>
      </el-col>
    </el-row>
    <!-- 按钮 -->
    <div class="operation">
      <el-row>
        <el-col :span='10'>
          <el-button>全选</el-button>

          <el-button>删除</el-button>
          <el-button>
            <span class="demonstration">请选择订单状态</span>
            <el-dropdown trigger="click">
              <span class="el-dropdown-link">
                &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
                <i class="el-icon-caret-bottom el-icon--right"></i>
              </span>
              <el-dropdown-menu slot="dropdown">
                <el-dropdown-item>未付款</el-dropdown-item>
                <el-dropdown-item>已付款待发货</el-dropdown-item>
                <el-dropdown-item>已发货待签署</el-dropdown-item>
                <el-dropdown-item>已签收</el-dropdown-item>

              </el-dropdown-menu>
            </el-dropdown>
          </el-button>

        </el-col>
        <el-col :span='4' :offset="10">
          <el-input placeholder="请输入搜索会员" icon="search" :on-icon-click="searchclick">
          </el-input>
        </el-col>
      </el-row>
    </div>
    <!-- 列表 -->
    <el-row>
      <el-col :span="24">

        <el-table :data="datalist" style="width: 100%" :row-class-name="tableRowClassName">

          <el-table-column type="selection" width="80">

            <!-- <template scope="scope">
              <el-checkbox v-model="checked"></el-checkbox>
            </template> -->
          </el-table-column>
          <el-table-column prop="order_no" label="订单号">
           <template scope='scope'>
              {{scope.row.order_no}}
           </template>
          </el-table-column>
          <el-table-column prop="user_name" label="会员账号" width="100">
          </el-table-column>
          <el-table-column prop="paymentTitle" label="支付方式" width="100">
          </el-table-column>
          <el-table-column prop="expressTitle" label="配送方式" width="100">
          </el-table-column>
          <el-table-column prop="statusName" label="订单状态" width="180">
          </el-table-column>
          <el-table-column prop="order_amount" label="总金额" width="80">
          </el-table-column>
          <el-table-column prop="add_time" label="下单时间" width="200">
            <template scope='scope'>
              {{scope.row.add_time|datefmt('YYYY-MM-DD HH:mm:ss')}}
            </template>
          </el-table-column>
          <el-table-column prop="id" label="操作" width="80">
            <template scope="scope">
              <router-link v-bind='{to:"/admin/orderedit/"+scope.row.id}'>
              <el-button type="success" size="mini">修改</el-button>
             </router-link>
            </template>
          </el-table-column>
        </el-table>
      </el-col>
    </el-row>
    <el-row>
      <el-col>
        <div class="block">
          <!-- <span class="demonstration">完整功能</span> -->
          <el-pagination @size-change="handleSizeChange"
           @current-change="handleCurrentChange" :current-page="currentPage1"
            :page-sizes="[10, 20, 30, 50,100,200]" :page-size="pageSize" layout="total, sizes, prev, pager, next, jumper" :total="totalcount">
          </el-pagination>
        </div>
      </el-col>
    </el-row>
  </div>
</template>
<script>
export default {
  data: function() {
    return {
      id: 0,
      datalist: [],
      currentPage1: 5,
      pageSize: 10,
    totalcount:0,
    pageIndex:1
      // checked: false
    }
  },
  created() {
    this.getdatalist()
  },
  methods: {
    //搜索会员
    searchclick(){

    },
    getdatalist() {
      var url = "/admin/order/getorderlist?orderstatus=&vipname=ivanyb&pageIndex="+this.pageIndex+"&pageSize=" + this.pageSize
      this.$http.get(url).then(res => {
        if (res.data.status == 1) {
          this.$message.error(res.data.message);
        }
        this.datalist = res.data.message;
        this.totalcount=res.data.totalcount;
      })
    },
    tableRowClassName(row, index) {
      if (index % 2 === 0) {
        return 'info-row';
      } else {
        return 'positive-row';
      }

    },
    //页码
    handleSizeChange(val) {
      console.log(`每页 ${val} 条`);
    },
    handleCurrentChange(val) {
      console.log(`当前页: ${val}`);
    }
  }
}
</script>