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
            @click="update(scope.row.companyId)"
            >修改</el-button
          >
          <el-button
            type="text"
            size="small"
            @click="deleteHandle(scope.row.companyId)"
            >删除</el-button
          >
        </template>
      </el-table-column>
    </el-table>
    <el-dialog
    :title="!dataForm.id ? '修改' : '修改'"
    :close-on-click-modal="false"
    :visible.sync="visible">
    <el-form
      :model="ruleForm"
      :rules="rules"
      ref="ruleForm"
      label-width="100px"
      class="demo-ruleForm"
    >
      <el-form-item label="公司名称" prop="name">
        <el-input v-model="ruleForm.name" style="width: 400px"></el-input>
      </el-form-item>
      <el-form-item label="岗位名称" prop="jobName">
        <el-input v-model="ruleForm.jobName" style="width: 400px"></el-input>
      </el-form-item>
      <el-form-item label="岗位描述" prop="jobDetail">
        <el-input type="textarea" v-model="ruleForm.jobDetail" style="width: 400px"></el-input>
      </el-form-item>
      <el-form-item label="岗位职责" prop="jobDuty">
        <el-input type="textarea" v-model="ruleForm.jobDuty" style="width: 400px"></el-input>
      </el-form-item>
      <el-form-item label="福利待遇" prop="money">
        <el-input type="textarea" v-model="ruleForm.money" style="width: 400px"></el-input>
      </el-form-item>
      <el-form-item label="应聘要求" prop="required">
        <el-input type="textarea" v-model="ruleForm.required" style="width: 400px"></el-input>
      </el-form-item>
      <el-form-item>
        <el-button type="primary" @click="submitForm('ruleForm')">保存</el-button>
        <el-button @click="resetForm('ruleForm')">重置</el-button>
      </el-form-item>
    </el-form>
  </el-dialog>
  
  </div>
  
</template>

<script>
export default {
  data () {
    return {
      dataForm: {
        companyId: '',
        jobName: ''
      },
      dataList: [],
      visible: false,
      ruleForm: {
        companyId: '',
        name: '',
        jobName: '',
        jobDetail: '',
        jobDuty: '',
        money: '',
        required: ''
      },
      rules: {
        name: [
          { required: true, message: '请输入公司名称', trigger: 'blur'},
          { min: 2, max: 30, message: '请输入公司完整名称', trigger: 'blur'},
        ],
        jobName: [
          { required: true, message: '请输入岗位名称', trigger: 'blur'}
        ],
        jobDetail: [
          { required: true, message: '请输入岗位描述', trigger: 'blur'}
        ],
        jobDuty: [
          { required: true, message: '请输入岗位职责', trigger: 'blur'}
        ],
        money: [
          { required: true, message: '请输入福利待遇', trigger: 'blur'}
        ],
        required: [
          { required: true, message: '请输入应聘要求', trigger: 'blur'}
        ],
      }
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
        url: this.$http.adornUrl("/company/getAllJobInfo"),
        method: "get",
        params: this.$http.adornParams({
          jobName: this.dataForm.jobName
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
    update (companyId) {
      console.log(companyId);
      this.ruleForm.companyId = companyId;
      this.visible = true;
    },
    deleteHandle(companyId) {
      this.$http({
        url: this.$http.adornUrl("/company/deleteById"),
        method: "post",
        params: {
          'companyId': companyId
        }
      }).then(({data}) => {
        if (data && data.code === 0) {
                this.$message({
                  message: '删除成功',
                  type: 'success',
                  duration: 1500,
                  onClose: () => {
                    this.visible = false
                  }
                });
                this.getDataList();
              } else {
                this.$message.error(data.msg)
              }
      });
    },
    submitForm (formName) {
      this.$refs['ruleForm'].$http({
              url: this.$http.adornUrl(`/company/updateCompanyJob`),
              method: 'post',
              params: {
                'companyId': this.ruleForm.companyId,
                'name': this.ruleForm.name,
                'jobName': this.ruleForm.jobName,
                'jobDetail': this.ruleForm.jobDetail,
                'jobDuty': this.ruleForm.jobDuty,
                'money': this.ruleForm.money,
                'required': this.ruleForm.required
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
                this.getDataList()
              } else {
                this.$message.error(data.msg)
              }
            })
    },
    resetForm (formName) {
      this.$refs[formName].resetFields()
    }
  }
};
</script>
