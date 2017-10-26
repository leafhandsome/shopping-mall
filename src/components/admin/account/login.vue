<template>
    <div class="accounttmpl">
        <el-form :model="from" :rules="rules" ref="form1">
            <el-row>
              <el-col :span="5" :offset='8'>
                <el-form-item label="用户名" prop="uname">
                  <el-input v-model="from.uname"></el-input>
                </el-form-item>
             
                  <el-form-item label="密码" prop="upwd">
                    <el-input v-model="from.upwd"></el-input>
                  </el-form-item>
              
                    <el-button type="success" @click="login">登录</el-button>

                </el-col>
              </el-row>
        </el-form>
    </div>
</template>

<script>
    export default {
        data() {
            return {
                from: {
                    uname: '',
                    upwd: ''
                },
                rules: {
                    uname: [{
                        required: true,
                        message: '请输入用户名',
                        trigger: 'blur'
                    }],
                    upwd: [{
                        required: true,
                        message: '请输入密码',
                        trigger: 'blur'
                    }],
                }
            }
        },
        //利用生命周期改变背景色
        mounted() {
            document.getElementsByTagName("body")[0].style.backgroundColor = "#0094ff"
        },
        beforeDestroy() {
            document.getElementsByTagName('body')[0].style.backgroundColor = "white"
        },
        methods: {

            login() {
                this.$refs.form1.validate(valid => {
                    if (valid) {
                        this.$http.post("/admin/account/login", this.from).then(res => {
                            if (res.data.status === 1) {
                                this.$message.error(res.data.message);
                                return;
                            }
                            this.$router.push({
                                name: "goodslist"
                            })
                        })
                    } else {
                        return false;
                    }
                })

            }

        }
    }
</script>
<style scoped>

</style>