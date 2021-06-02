<template>
  <div>
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
  </div>
</template>

<script>
export default {
  data() {
    return {
      ruleForm: {
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
    }
  },
  methods: {
    submitForm (formName) {
      if (this.ruleForm.name == '') {
            this.$message({type: 'warning', message: '请输入公司名称', showClose: true});
            return;
      }
      if (this.ruleForm.jobName == '') {
            this.$message({type: 'warning', message: '请输入岗位名称', showClose: true});
            return;
      }
      if (this.ruleForm.jobDetail == '') {
            this.$message({type: 'warning', message: '请输入岗位描述', showClose: true});
            return;
      }
      if (this.ruleForm.jobDuty == '') {
            this.$message({type: 'warning', message: '请输入岗位职责', showClose: true});
            return;
      }
      if (this.ruleForm.money == '') {
            this.$message({type: 'warning', message: '请输入福利待遇', showClose: true});
            return;
      }
      if (this.ruleForm.required == '') {
            this.$message({type: 'warning', message: '请输入应聘要求', showClose: true});
            return;
      }
      this.$refs['ruleForm'].$http({
              url: this.$http.adornUrl(`/company/saveCompanyJob`),
              method: 'post',
              params: {
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
                })
              } else {
                this.$message.error(data.msg)
              }
            })
    },
    resetForm (formName) {
      this.$refs[formName].resetFields()
    }
  }
}
</script>