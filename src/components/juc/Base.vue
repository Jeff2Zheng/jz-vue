<template>
  <div>
    <!-- 导航区 -->
    <el-breadcrumb separator-class="el-icon-arrow-right">
      <el-breadcrumb-item :to="{ path: '/home' }">首页</el-breadcrumb-item>
      <el-breadcrumb-item>JUC</el-breadcrumb-item>
      <el-breadcrumb-item>JUC列表</el-breadcrumb-item>
    </el-breadcrumb>
    <!-- 搜索,功能实现区域 -->
    <el-card>
      <el-row :gutter="20">
        <el-col :span="6">
          <el-input placeholder="可根据名称进行检索" v-model="queryInfo.jucName" clearable @clear="getJucList">
            <el-button slot="append" size="mini" icon="el-icon-search" @click="getJucList"></el-button>
          </el-input>
        </el-col>
        <el-col :span="4">
          <el-button type="primary" @click="addJucFlag = true">添加</el-button>
        </el-col>
      </el-row>
      <!-- 用户列表区域 -->
      <el-table :data="userlist" border stripe>
        <el-table-column prop="jucName" align="center" width="220" label="名称"></el-table-column>
        <el-table-column prop="jucType" align="center" width="140" label="所属类型" :formatter="jucTypeDict"></el-table-column>
        <el-table-column prop="jucImportant" align="center" width="140" label="重要程度" :formatter="jucImportantDict"></el-table-column>
        <el-table-column prop="jucUrl" align="center" width="500" label="外部路径">
          <template slot-scope="scope">
            <a :href="scope.row.jucUrl" target="_blank" class="buttonText">{{scope.row.jucUrl}}</a>
          </template>
        </el-table-column>
        <el-table-column label="操作">
          <template slot-scope="scope">
            <el-button
              type="primary"
              size="mini"
              @click="detail(scope.row.id)"
            >查看</el-button>
            <el-button
              type="primary"
              size="mini"
              @click="downLoad(scope.row.id)"
            >API文档下载</el-button>
          </template>
        </el-table-column>
      </el-table>
      <!-- 分页区域 -->
      <el-pagination
        @size-change="handleSizeChange"
        @current-change="handleCurrentChange"
        :current-page="queryInfo.current"
        :page-sizes="[2, 5, 10, 15]"
        :page-size="queryInfo.size"
        layout="total, sizes, prev, pager, next, jumper"
        :total="total"
      ></el-pagination>
    </el-card>

    <!-- ====================================================新增弹出=========================================================-->
    <el-dialog title="新增" :visible.sync="addJucFlag" width="50%" >
      <!-- 内容主体 -->
      <el-form
        ref="addUserFormRef"
        label-width="100px"
      >
        <el-form-item label="用户名" prop="username">
          <el-input ></el-input>
        </el-form-item>
      </el-form>
      <span slot="footer" class="dialog-footer">
        <el-button @click="addJucFlag = false">取 消</el-button>
        <el-button type="primary" @click="addJuc">确 定</el-button>
      </span>
    </el-dialog>

  </div>
</template>
<script>
  export default {
    data () {
      return {
        // ====================================================分页区域=========================================================
        queryInfo: {
          jucName: null,
          current: 1,
          size: 5
        },
        userlist: [],
        total: 0,
        // ====================================================新增功能区域=========================================================
        addJucFlag: false,


      }
    },
    created () {
      this.getJucList()
    },
    methods: {
      // ====================================================分页区域=========================================================
      async getJucList() {
        const { data: res } = await this.$http.post('api-user/api/juc/page', this.queryInfo)
        if (res.code !== 200) {
          return this.$message.error('获取列表失败，请稍后重试！')
        }
        //列表数据库
        this.userlist = res.data.records;
        //总页数
        this.total = res.data.total
      },
      handleSizeChange (newSize) {
        this.queryInfo.size = newSize;
        this.getJucList()
      },
      handleCurrentChange (newSize) {
        this.queryInfo.current = newSize;
        this.getJucList()
      },
      // ====================================================详情页=========================================================
      async detail(){
        return this.$message.error('该功能还未实现！')
      },
      // ====================================================新增功能区域=========================================================
      async addJuc(){
        return this.$message.error('该功能还未实现！')
      },
      // ====================================================示例文档下载=========================================================
      async downLoad(){
        return this.$message.error('该功能还未实现！')
      },
      // ====================================================新增功能区域=========================================================
      // ====================================================字典统一处理=========================================================
      jucImportantDict(row){
        if(row.jucImportant=='20'){
          return "中"
        } else if(row.jucImportant=='30'){
          return "低"
        }else{
          return "高"
        }
      },
      jucTypeDict(row){
        if(row.jucType=='10'){
          return "JUC锁"
        }
      },


    }
  }
</script>

<style lang="less" scoped>
</style>
