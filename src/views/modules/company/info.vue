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
        <el-input v-model="ruleForm.name" style="width: 200px"></el-input>
      </el-form-item>
      <el-form-item label="联系方式" prop="phone">
        <el-input v-model="ruleForm.phone" style="width: 200px"></el-input>
      </el-form-item>
      <el-form-item label="公司地址" prop="address">
        <el-input v-model="ruleForm.address" style="width: 200px"></el-input>
      </el-form-item>
      <el-form-item label="邮箱" prop="email">
        <el-input v-model="ruleForm.email" style="width: 200px"></el-input>
      </el-form-item>
      <el-form-item label="公司简介" prop="detail">
        <el-input
          type="textarea"
          v-model="ruleForm.detail"
          style="width: 600px"
        ></el-input>
      </el-form-item>
      <el-form-item>
        <el-button type="primary" @click="submitForm()"
          >立即创建</el-button
        >
        <el-button @click="resetForm('ruleForm')">重置</el-button>
      </el-form-item>
    </el-form>
  </div>
</template>

<script>
export default {
  data() {
    let isEmail = (rule, value, callback) => {
      let reg = new RegExp(/^([a-zA-Z0-9_-])+@([a-zA-Z0-9_-])+((.[a-zA-Z0-9_-]{2,3}){1,2})$/);
      console.log(reg.test(value));
      if (!reg.test(value)) {
         callback(new Error('请输入正确的邮箱'));
      }
    }
    return {
      ruleForm: {
        name: '',
        phone: '',
        address: '',
        email: '',
        detail: '',
      },
      rules: {
        name: [
          { required: true, message: '请输入公司名称', trigger: 'blur'},
          { min: 2, max: 30, message: '请输入公司完整名称', trigger: 'blur'},
        ],
        phone: [{ required: true, message: '请输入联系方式', trigger: 'blur'}],
        address: [
          { required: true, message: '请输入公司地址', trigger: 'blur'}
        ],
        email: [
          { required: true, message: '请输入邮箱', trigger: 'blur' },
          {validator: isEmail}
        ]
      }
    }
  },
  methods: {
    submitForm () {
      this.$refs['ruleForm'].$http({
              url: this.$http.adornUrl(`/company/saveCompanyInfo`),
              method: 'post',
              params: {
                'name': this.ruleForm.name,
                'phone': this.ruleForm.phone,
                'address': this.ruleForm.address,
                'email': this.ruleForm.email,
                'detail': this.ruleForm.detail 
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
                this.resetForm('ruleForm');
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