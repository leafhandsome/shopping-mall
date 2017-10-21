<template>
    <div class="arttmpl">
        <el-row>
            <el-col :span="24">
                <div class="abread bt-line">
                    <!-- 使用elementUI中的面包屑导航组件完成 -->
                    <el-breadcrumb separator="/">
                        <el-breadcrumb-item :to="{ path: '/admin' }">首页</el-breadcrumb-item>
                        <el-breadcrumb-item>商品管理</el-breadcrumb-item>
                        <el-breadcrumb-item>商品列表</el-breadcrumb-item>
                    </el-breadcrumb>
                </div>
            </el-col>
        </el-row>
        <div class="operation">
            <el-row>
                <el-col :span="5">
                    <!-- 新增，删除，全选按钮 -->
                    <el-button >全选</el-button>
                    <router-link to="/admin/goodsadd">
                        <el-button>新增</el-button>
                    </router-link>
                    <el-button>删除</el-button>
                </el-col>
                <el-col :span="3" :offset="16">
                    <!-- 搜索框 -->
                    <el-input placeholder="请输入搜索条件" icon="search" v-model="searchValue" :on-icon-click="getlist">
                    </el-input>
                </el-col>
            </el-row>
        </div>

        <el-row>
            <el-col :span="24">
                <el-table :data="list" style="width: 100%" :row-class-name="tableRowClassName" @selection-change="getrows">
                    <el-table-column type='selection' width="80">
                    </el-table-column>
                    <el-table-column prop="title" label="标题">
                        <template scope='scope'>
                            <router-link v-bind="{to:'/admin/goodsedit/'+scope.row.id}">
                                {{scope.row.title}}
                            </router-link>

                        </template>
                    </el-table-column>
                    <el-table-column prop="categoryname" label="类别" width="100">
                    </el-table-column>
                    <el-table-column label="发布人/发布时间" width="200">
                        <template scope="scope">
                            {{scope.row.user_name }} / {{scope.row.add_time}}
                        </template>
                    </el-table-column>
                    <el-table-column prop="name" label="属性" width="180">
                        <template scope="scope">
                           
                             <el-tooltip class="item" effect="dark" v-bind='{content:(scope.row.is_slide==1?"轮播":"不轮播")}' placement="bottom">
                               <i v-bind='{class:"el-icon-picture ls"+(scope.row.is_slide==1?"imgactive":"")}'></i>
                            </el-tooltip>
                           
                            <el-tooltip class="item" effect="dark" v-bind="{content:(scope.row.is_top==1?'置顶':'不置顶')}" placement="bottom">
                                <i v-bind="{class:'el-icon-upload ls'+(scope.row.is_top==1?'imgactive':'')}"></i>
                            </el-tooltip>
                            <el-tooltip class="item" effect="dark" v-bind='{content:(scope.row.is_hot==1? "热门":"非热门")}' placement="bottom">
                               <i v-bind='{class:"el-icon-star-on ls"+(scope.row.is_hot==1? "imgactive":"")}'></i>
                            </el-tooltip>
                           
                        </template>
                    </el-table-column>
                    <el-table-column label="操作" width="80">
                        <template scope="scope">
                            <router-link v-bind="{to:'/admin/goodsedit/'+scope.row.id}">
                                <el-button type="success" size="mini">修改</el-button>
                            </router-link>

                            <!-- <a v-bind='{href:"./#/admin/goodsedit/"+id}'>修改</a> -->
                        </template>
                    </el-table-column>
                </el-table>
            </el-col>
        </el-row>
        <el-row>
            <el-col>
                <div class="block">
                    <!-- <span class="demonstration">完整功能</span> -->
                    <el-pagination @size-change="handleSizeChange" @current-change="handleCurrentChange" :current-page="currentPage4" :page-sizes="[10, 20, 30, 50,100,200]" :page-size="pagesize" layout="total, sizes, prev, pager, next, jumper" :total="total">
                    </el-pagination>
                </div>
            </el-col>
        </el-row>
    </div>
</template>


<script>
export default {
    data() {
        return {
            ids: "",
            // 搜索框的绑定属性
            searchValue: '',
            // 表格中的每行数据来源于list，而这个list将来是通过getlist()方法请求后台api接口获取到的
            list: [],

            pagesize: 10,
            pageindex: 1,

            currentPage4: 4,
            total: 0
        }
    },
    created() {
        // 获取到列表数据
        this.getlist();
    },

    methods: {
        // 用axios去发出具体的url的请求获取到数据后绑定到表格中
        getlist() {
            // 1.0 获取url
            var url = "/admin/goods/getlist?pageIndex=" + this.pageindex + "&pageSize=" + this.pagesize + "&searchvalue=" + this.searchValue;
            this.$http.get(url).then(res => {
                // 判断服务器返回的状态status
                if (res.data.status == 1) {
                    this.$message.error(res.data.message);
                    return;
                }

                // 正常逻辑的处理
                this.total = res.data.totalcount;
                this.list = res.data.message;
            });
        },
        //勾选的对象rows
        getrows(rows) {
            this.ids = "";
            var idscart = ",";
            for (var i = 0; i < rows.length; i++) {
                if (i == rows.length - 1) {
                    idscart = ""
                }
                this.ids += rows[i].id + idscart
            }
            // console.log(this.ids);

        },
        // 控制表格的隔行变色
        tableRowClassName(row, index) {
            //    控制奇数和偶数行的颜色
            if (index % 2 == 0) {
                return 'info-row';
            } else {
                return 'positive-row';
            }
        },
        //页码
        handleSizeChange(currentpage) {
            // console.log(`每页 ${val} 条`);
            this.pagesize = currentpage;
            this.getlist();
        },
        handleCurrentChange(val) {
            // console.log(`当前页: ${val}`);
            this.pageindex = val;
            this.getlist()
        }
    }

}
</script>
<style scoped>

</style>