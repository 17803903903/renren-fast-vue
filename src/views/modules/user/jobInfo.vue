<template>
  <div class="mod-user">
    <el-form
      :inline="true"
      :model="dataForm"
      @keyup.enter.native="getDataList()"
    >
      <el-form-item>
        <el-input
          v-model="dataForm.jobName"
          placeholder="岗位名称"
          clearable
        ></el-input>
      </el-form-item>
      <el-form-item>
        <el-input
          v-model="dataForm.name"
          placeholder="公司名称"
          clearable
        ></el-input>
      </el-form-item>
      <el-form-item>
        <el-button @click="getDataList()">查询</el-button>
      </el-form-item>
    </el-form>
    <el-table
      :data="dataList"
      border
      style="width: 100%"
    >
      <el-table-column
        prop="companyId"
        header-align="center"
        align="center"
        width="80"
        label="ID"
      >
      </el-table-column>
      <el-table-column
        prop="jobName"
        header-align="center"
        align="center"
        label="岗位名称"
      >
      </el-table-column>
      <el-table-column
        prop="name"
        header-align="center"
        align="center"
        label="公司名称"
      >
      </el-table-column>
      <el-table-column
        prop="jobDetail"
        header-align="center"
        align="center"
        label="岗位描述"
      >
      </el-table-column>
      <el-table-column
        prop="jobDuty"
        header-align="center"
        align="center"
        label="岗位职责"
      >
      </el-table-column>
      <el-table-column
        prop="money"
        header-align="center"
        align="center"
        label="福利待遇"
      >
      </el-table-column>
      <el-table-column
        prop="required"
        header-align="center"
        align="center"
        label="应聘要求"
      >
      </el-table-column>
      <el-table-column
        fixed="right"
        header-align="center"
        align="center"
        width="150"
        label="操作"
      >
        <template slot-scope="scope">
          <el-button
            type="text"
            size="small"
            @click="findJob(scope.row.companyId)"
            >投递简历</el-button
          >
        </template>
      </el-table-column>
    </el-table>
  </div>
</template>

<script>
export default {
  data () {
    return {
      dataForm: {
        companyId: '',
        jobName: '',
        name: ''
      },
      dataList: []
    };
  },
  activated() {
      this.dataList = [];
      this.getDataList();
  },
  methods: {
    // 获取数据列表
    getDataList() {
      this.$http({
        url: this.$http.adornUrl("/user/getAllJobInfo"),
        method: "get",
        params: this.$http.adornParams({
          jobName: this.dataForm.jobName,
          name: this.dataForm.name
        }),
      }).then(({ data }) => {
        if (data && data.code === 0) {
          this.dataList = data.jobInfo;
          console.log(this.dataList)
        } else {
          this.$message.error(data.msg)
        }
      });
    },
    findJob(companyId) {
      console.log(companyId);
      this.$http({
              url: this.$http.adornUrl(`/user/saveCompanyPost`),
              method: 'post',
              params: {
                'companyId': companyId
              }
            }).then(({data}) => {
              if (data && data.code === 0) {
                this.$message({
                  message: '操作成功',
                  type: 'success',
                  duration: 1500,
                  onClose: () => {
                    this.visible = false
                  }
                });
              } else {
                this.$message.error(data.msg)
              }
            })
    }
  },
};
</script>
