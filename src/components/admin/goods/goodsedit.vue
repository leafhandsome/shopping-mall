<template>
  <div class="arttmpl">
    <el-row>
      <el-col>
        <div class="abread bt-line">
          <!-- //面包屑 -->
          <el-breadcrumb separator="/">
            <el-breadcrumb-item :to="{ path: '/admin/orderlist' }">返回上一层 </el-breadcrumb-item>
            <el-breadcrumb-item>购物商城</el-breadcrumb-item>
            <el-breadcrumb-item>首页</el-breadcrumb-item>
            <el-breadcrumb-item>编辑商品</el-breadcrumb-item>

          </el-breadcrumb>
        </div>
      </el-col>
    </el-row>
    <!-- //表单 -->
    <el-form :model="ruleForm" :rules="rules" ref="ruleForm" label-width="100px" class="demo-ruleForm">
      <el-row>
        <el-col :span="10">
          <el-form-item label="标题" prop="title">
            <el-input v-model="ruleForm.title"></el-input>
          </el-form-item>
        </el-col>
        <el-col :span='10'>
          <el-form-item label="副标题" prop="sub_title">
            <el-input v-model="ruleForm.sub_title"></el-input>
          </el-form-item>
        </el-col>
      </el-row>
      <el-row>
        <el-col :span="10" >
           <el-form-item label="所属类别" prop="category_id">
          <el-select v-model="ruleForm.category_id" placeholder="请选择">
        <el-option v-for="(cate,index) in categorylist" :key="index" :label="cate.title" :value="cate.category_id">
          
        </el-option>
      </el-select>
       </el-form-item>
        </el-col>
      </el-row>
     
        <el-form-item>
        <el-button type="primary" @click="submitForm('ruleForm')">立即创建</el-button>
        <el-button @click="resetForm('ruleForm')">重置</el-button>
      </el-form-item>
    </el-form>
  </div>
</template>
<script>
    export default {
        data() {
            return {
                categorylist: [],
                ruleForm: {
                    title: '',
                    sub_title: '',
                    category_id: ''
                },
                rules: {
                    title: [{
                        required: true,
                        message: '请输入表题',
                        trigger: 'blur'
                    }]
                }
            };
        },
        created() {
            this.category();
        },
        methods: {
            //所属类别
            category() {
                this.$http.get('/admin/category/getlist/goods').then(res => {
                    if (res.data.status == 1) {
                        this.$message.error(res.data.message);
                        return;
                    }
                    this.categorylist = res.data.message;
                })
            },
            submitForm(formName) {
                this.$refs[formName].validate((valid) => {
                    if (valid) {
                        //发送ajax请求
                    } else {
                        console.log('error submit!!');
                        return false;
                    }
                });
            },
            resetForm(formName) {
                this.$refs[formName].resetFields();
            }
        }
    }
</script>