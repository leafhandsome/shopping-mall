<template>
  <div class="arttmpl">
    <el-row>
      <el-col>
        <div class="abread bt-line">
          <!-- //面包屑 -->
          <el-breadcrumb separator="/">
            <el-breadcrumb-item :to="{ path: '/admin/orderlist' }">返回上一层 </el-breadcrumb-item>
            <el-breadcrumb-item>订单管理</el-breadcrumb-item>
            <el-breadcrumb-item>首页</el-breadcrumb-item>
            <el-breadcrumb-item>编辑订单</el-breadcrumb-item>

          </el-breadcrumb>
        </div>
      </el-col>
    </el-row>
    <el-form ref="form" :model="form" label-width="80px">

      <el-row>
        <el-col :span="10" offset="1">
          <div style="margin: 20px;"></div>

          <el-form-item label="订单号">
          123456
          </el-form-item>
          <el-form-item label="收件人">
            <el-input v-model="form.name"></el-input>
          </el-form-item>
          <el-form-item label="发货地区">
            <el-input v-model="form.region"></el-input>
          </el-form-item>
          <el-form-item label="手机">
            <el-input v-model="form.type"></el-input>
          </el-form-item>
          <el-form-item label="邮箱">
            <el-input v-model="form.type"></el-input>
          </el-form-item>
          <el-form-item label="总金额">
            <el-input v-model="form.type"></el-input>
          </el-form-item>

        </el-col>
        <div style="margin: 20px;"></div>
        <el-col :span="10">
          <el-form-item label="订单状态">
            <el-select v-model="form.region" placeholder="已发货待签收">
              <el-option label="待付款" value="shanghai"></el-option>
              <el-option label="已付款待发货" value="beijing"></el-option>
              <el-option label="已发货待签收" value="guanzhou"></el-option>
              <el-option label="已签收" value="shengzhen"></el-option>
              <el-option label="已取消" value="changsha"></el-option>
            </el-select>
          </el-form-item>
          <div style="margin: 20px;"></div>

          <el-form-item label="物流单号">
            <el-input v-model="form.name"></el-input>
          </el-form-item>
          <el-form-item label="详细地址">
            <el-input v-model="form.address"></el-input>
          </el-form-item>
          <el-form-item label="电话">
            <el-input v-model="form.type"></el-input>
          </el-form-item>
          <el-form-item label="订单留言">
            <el-input v-model="form.message"></el-input>
          </el-form-item>
          <el-form-item label="配送费">
            <el-input v-model="form.type"></el-input>
          </el-form-item>

        </el-col>
      </el-row>
      <el-row>
        <el-col :span="2">
          <el-form-item label="商品列表">
          </el-form-item>
        </el-col>
        <el-col :span="19">
          <el-table :data="tableData" style="width: 100%">
            <el-table-column prop="date" label="日期" width="180">
            </el-table-column>
            <el-table-column prop="name" label="姓名" width="180">
            </el-table-column>
            <el-table-column prop="address" label="地址">
            </el-table-column>
          </el-table>
        </el-col>
      </el-row>
      <el-form-item>
        <el-button type="primary" @click="onSubmit">提交保存</el-button>
        <el-button :to="{ path: '/admin/orderlist' }">返回上一层</el-button>
      </el-form-item>
    </el-form>
  </div>
</template>
<script>
export default {
  data() {
    return {
      form: {
        order_no:"BD20171007134225417",

        name: '',
        region: '',
        date1: '',
        date2: '',
        delivery: false,
        type: [],
        resource: '',
        desc: '',

      },
      labelPosition: 'right',
    }
  },
  methods: {
    //数据回显

    onSubmit() {
      console.log('submit!');
    }
  }

}
</script>