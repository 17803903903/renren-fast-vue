<template>
  <div>
    <el-form
      :model="ruleForm"
      :rules="rules"
      ref="ruleForm"
      label-width="100px"
      class="demo-ruleForm"
    >
      <el-form-item label="姓名" prop="name">
        <el-input v-model="ruleForm.name" style="width: 200px"></el-input>
      </el-form-item>
      <el-form-item label="联系方式" prop="phone">
        <el-input v-model="ruleForm.phone" style="width: 200px"></el-input>
      </el-form-item>
      <el-form-item label="邮箱" prop="email">
        <el-input v-model="ruleForm.email" style="width: 200px"></el-input>
      </el-form-item>
      <el-form-item label="地址" prop="address">
        <el-input v-model="ruleForm.address" style="width: 200px"></el-input>
      </el-form-item>
      <el-form-item label="家庭信息" prop="family">
        <el-input
          type="textarea"
          v-model="ruleForm.family"
          style="width: 600px"
        ></el-input>
      </el-form-item>
      <el-form-item label="教育背景" prop="education">
        <el-input
          type="textarea"
          v-model="ruleForm.education"
          style="width: 600px"
        ></el-input>
      </el-form-item>
      <el-form-item label="工作经验" prop="work">
        <el-input
          type="textarea"
          v-model="ruleForm.work"
          style="width: 600px"
        ></el-input>
      </el-form-item>
      <el-form-item label="个人描述" prop="description">
        <el-input
          type="textarea"
          v-model="ruleForm.description"
          style="width: 600px"
        ></el-input>
      </el-form-item>
      <el-form-item>
        <el-button type="primary" @click="formSubmit()">保存</el-button>
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
        email: '',
        address: '',
        family: '',
        education: '',
        work: '',
        description: '',
      },
      rules: {
        name: [
          { required: true, message: '请输入姓名', trigger: 'blur'},
          { min: 2, max: 5, message: '长度在 2 到 5 个字符', trigger: 'blur'},
        ],
        phone: [{ required: true, message: '请输入联系方式', trigger: 'blur'}],
        address: [
          { required: true, message: '请输入地址', trigger: 'blur'}
        ],
        email: [
          { required: true, message: '请输入邮箱', trigger: 'blur' },
          {validator: isEmail}
        ]
      }
    }
  },
  methods: {
    formSubmit () {
        this.$refs['ruleForm'].$http({
              url: this.$http.adornUrl(`/user/updateUserInfo`),
              method: 'post',
              params: {
                'name': this.ruleForm.name,
                'phone': this.ruleForm.phone,
                'email': this.ruleForm.email,
                'address': this.ruleForm.address,
                'family': this.ruleForm.family,
                'education': this.ruleForm.education,
                'work': this.ruleForm.work,
                'description': this.ruleForm.description
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
    resetForm (ruleForm) {
      this.$refs[ruleForm].resetFields()
    }
  }
}
</script>