<template>
    <div class="arttmpl">
      <el-row>
        <el-col>
          <div class="abread bt-line">
            <!-- //面包屑 -->
            <el-breadcrumb separator="/">
              <el-breadcrumb-item :to="{ path: '/admin/goodslist' }">返回上一层 </el-breadcrumb-item>
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
          <el-col :span="8">
            <el-form-item label="类别" prop="category_id">
              <el-select v-model="ruleForm.category_id" placeholder="请选择">
                <el-option v-for="(cate,index) in categorylist" :key="index"
                 :label="cate.title" :value="cate.category_id">
                  <span v-for="count in (cate.class_layer-1)">&nbsp;</span>
                  <span v-if="cate.class_layer>1">|-</span>
                  <span v-text="cate.title"></span>
                </el-option>
              </el-select>
            </el-form-item>
          </el-col>
          <el-col :span="5">
            <el-form-item label="是否发布">
              <el-switch :width="80" v-model="ruleForm.is_status" on-text="发布" off-text="不发布">
              </el-switch>
            </el-form-item>
  
          </el-col>
          <el-col :span="11">
            <el-form-item label="推荐类型">
              <el-switch :width="80" v-model="ruleForm.is_slide" on-text="轮播" off-text="不轮播">
              </el-switch>
              <el-switch :width="80" v-model="ruleForm.is_top" on-text="置顶" off-text="不置顶">
              </el-switch>
              <el-switch :width="80" v-model="ruleForm.is_hot" on-text="热门" off-text="非热门">
              </el-switch>
            </el-form-item>
  
          </el-col>
        </el-row>
     
        <!-- 数量 -->
        <el-row>
          <el-col :span="5">
            <el-form-item label="商品货号" prop="goods_no">
              <el-input v-model="ruleForm.goods_no"></el-input>
            </el-form-item>
          </el-col>
          <el-col :span="5">
            <el-form-item label="库存数量" prop="stock_quantity">
              <el-input v-model="ruleForm.stock_quantity"></el-input>
            </el-form-item>
          </el-col>
          <el-col :span="5">
            <el-form-item label="市场价格" prop="market_price">
              <el-input v-model="ruleForm.market_price"></el-input>
            </el-form-item>
          </el-col>
          <el-col :span="5">
            <el-form-item label="销售价格" prop="sell_price">
              <el-input v-model="ruleForm.sell_price"></el-input>
            </el-form-item>
          </el-col>
  
        </el-row>
  
        <!-- 摘要 -->
        <el-row>
          <el-col :span="20">
            <el-form-item label="内容摘要" prop="zhaiyao">
              <el-input type="textarea" v-model="ruleForm.zhaiyao"></el-input>
            </el-form-item>
          </el-col>
  
        </el-row>
        <!-- 详情 -->
        <el-row :height="400">
          <el-col :span="20">
            <el-form-item label="详细描述">
              <quill-editor v-model="ruleForm.content"></quill-editor>
            </el-form-item>
          </el-col>
        </el-row>
        <el-form-item>
          <el-button type="primary" @click="submitForm('ruleForm')">保存</el-button>
          <el-button @click="resetForm('ruleForm')">重置</el-button>
        </el-form-item>
      </el-form>
    </div>
  </template>
<script>
    //导入富文本编辑器
    import {
        quillEditor
    } from 'vue-quill-editor'
    export default {
        components: {
            // 注册组件
            quillEditor
        },
        data() {

            var checkNumber = (rule, value, callback) => {
                // 非空验证
                if (!value) {
                    return callback(new Error('不能为空'));
                }
                if (isNaN(value)) {
                    return callback(new Error('输入的值必须是一个数字'));
                } else {
                    callback();
                }
            };
            return {
                categorylist: [],
                ruleForm: {
                    title: '',
                    sub_title: '',
                    category_id: 0,
                    status: true,
                    is_slide: true,
                    is_top: true,
                    is_hot: true,
                    goods_no: '',
                    stock_quantity: 0,
                    market_price: 0,
                    sell_price: 0,
                    zhaiyao: "",
                    content: '',
                    imgList: [],
                    fileList: []
                },
                rules: {
                    title: [{
                        required: true,
                        message: '请输入表题',
                        trigger: 'blur'
                    }],
                    goods_no: [{
                        required: true,
                        message: '请商品号',
                        trigger: 'blur'
                    }],
                    stock_quantity: [{
                        validator: checkNumber,
                        trigger: 'blur'
                    }],

                    market_price: [{
                        validator: checkNumber,
                        trigger: 'blur'
                    }],
                    sell_price: [{
                        validator: checkNumber,
                        trigger: 'blur'
                    }]
                }
            };
        },
        created() {
            this.category();
           
        },
        methods: {
            //数据回显http://139.199.192.48:6060/admin/goods/edit/107 ri 骗我感情
            getedit() {

                this.$http.get('/admin/goods/getgoodsmodel/' + this.$route.params.id).then(res=>{
                    if(res.data.status==1){
                        this.$message.error(res.data.message);
                        return;
                    }
                    this.ruleForm=res.data.message;
                    this.ruleForm.category_id=(res.data.message.category_id-0)
                })
            },
            // 上传图片

            imguploaded(response, file, fileList) {
                console.log(response);

                this.ruleForm.imgList = [response]
            },
            fileuploaded(response, file, fileList) {
                this.ruleForm.fileList.push(response)
            },
            //所属类别
            category() {
                this.$http.get('/admin/category/getlist/goods').then(res => {
                    if (res.data.status == 1) {
                        this.$message.error(res.data.message);
                        return;
                    }
                    this.categorylist = res.data.message;
                     this.getedit();
                })
            },
            submitForm(formName) {
                this.$refs[formName].validate((valid) => {
                    if (valid) {
                        //发送ajax请求
                        console.log(this.ruleForm);

                        this.$http.post('/admin/goods/add/goods', this.ruleForm).then(res => {
                            if (res.data.status == 1) {
                                this.$message.error("请求失败")
                                return;
                            }
                            this.$router.push({
                                name: 'goodslist'
                            });
                        })
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