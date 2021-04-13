<template>
  <div>
    <el-form
      :model="ruleForm"
      :rules="rules"
      ref="ruleForm"
      label-width="100px"
      class="demo-ruleForm"
    >
      <el-form-item label="公司名称" prop="companyName">
        <el-input v-model="ruleForm.companyName" style="width: 200px"></el-input>
      </el-form-item>
      <el-form-item label="联系方式" prop="phone">
        <el-input v-model="ruleForm.phone" style="width: 200px"></el-input>
      </el-form-item>
      <el-form-item label="验证码" prop="code">
        <el-input v-model="ruleForm.code" style="width: 100px"></el-input>
        <el-button type="primary" @click="getCode()">获取验证码</el-button>
      </el-form-item>
      <el-form-item label="公司联系人身份证号" prop="idNumber">
        <el-input v-model="ruleForm.idNumber" style="width: 200px"></el-input>
      </el-form-item>
      <el-form-item label="身份证照片" prop="photos">
        <el-upload
          action="https://jsonplaceholder.typicode.com/posts/"
          list-type="picture-card"
          :on-preview="handlePictureCardPreview"
          :on-remove="handleRemove"
          :on-change="addPhoto"
          v-model="ruleForm.photos"
        >
        <i class="el-icon-plus"></i>
        </el-upload>
        <el-dialog :visible.sync="ruleForm.dialogVisible">
          <img width="100%" :src="ruleForm.dialogImageUrl" alt="" />
        </el-dialog>
      </el-form-item>

      <el-form-item>
        <el-button type="primary" @click="submitForm()">认证</el-button>
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
        companyName: '',
        phone: '',
        code: '',
        idNumber: '',
        photos: [],
        dialogImageUrl: '',
        dialogVisible: false,
      },
      rules: {
        companyName: [
          { required: true, message: "请输入公司名称", trigger: "blur" },
          { min: 2, max: 30, message: "请输入公司完整名称", trigger: "blur" },
        ],
        phone: [{ required: true, message: "请输入联系方式", trigger: "blur" }],
        code: [{ required: true, message: "请输入验证码", trigger: "blur" }],
      },
    };
  },
  methods: {
    submitForm () {
      this.$refs['ruleForm'].$http({
              url: this.$http.adornUrl(`/company/certification`),
              method: 'post',
              params: {
                'name': this.ruleForm.companyName,
                'phone': this.ruleForm.phone,
                'code': this.ruleForm.code,
                'idNumber': this.ruleForm.idNumber
              }
            }).then(({data}) => {
              if (data && data.code === 0) {
                this.$message({
                  message: '认证成功',
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
    },
    handleRemove (file, fileList) {
      console.log(file, fileList)
    },
    handlePictureCardPreview (file) {
      this.ruleForm.dialogImageUrl = file.url
      this.ruleForm.dialogVisible = true
    },
    addPhoto (file) {
      console.log(file.url)
      let url = file.url
      let urls = []
      urls.push(url)
      this.ruleForm.photos = urls
    },
    getCode () {
      this.$http({
        url: this.$http.adornUrl(`/user/code/${this.ruleForm.phone}`),
        method: 'get',
        params: this.$http.adornParams()
      }).then(({data}) => {
        if (data && data.code === 0) {
          this.$message.success(data.msg)
        } else {
          this.$message.error(data.msg)
        }
      })
    }
  }
}
</script>

<style>
.avatar-uploader .el-upload {
  border: 1px dashed #d9d9d9;
  border-radius: 6px;
  cursor: pointer;
  position: relative;
  overflow: hidden;
}
.avatar-uploader .el-upload:hover {
  border-color: #409eff;
}
.avatar-uploader-icon {
  font-size: 28px;
  color: #8c939d;
  width: 178px;
  height: 178px;
  line-height: 178px;
  text-align: center;
}
.avatar {
  width: 178px;
  height: 178px;
  display: block;
}
</style>