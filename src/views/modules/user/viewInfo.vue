<template>
  <div class="mod-user">
    <el-table
      :data="dataList"
      border
      style="width: 100%"
    >
      <el-table-column
        prop="userId"
        header-align="center"
        align="center"
        width="80"
        label="ID"
      >
      </el-table-column>
      <el-table-column
        prop="name"
        header-align="center"
        align="center"
        label="姓名"
      >
      </el-table-column>
      <el-table-column
        prop="phone"
        header-align="center"
        align="center"
        label="电话号码"
      >
      </el-table-column>
      <el-table-column
        prop="date"
        header-align="center"
        align="center"
        label="出生日期"
      >
      </el-table-column>
      <el-table-column
        prop="email"
        header-align="center"
        align="center"
        label="邮箱"
      >
      </el-table-column>
      <el-table-column
        prop="address"
        header-align="center"
        align="center"
        label="地址"
      >
      </el-table-column>
      <el-table-column
        prop="idNumber"
        header-align="center"
        align="center"
        label="身份证号"
      >
      </el-table-column>
      <el-table-column
        prop="family"
        header-align="center"
        align="center"
        label="家庭信息"
      >
      </el-table-column>
      <el-table-column
        prop="education"
        header-align="center"
        align="center"
        label="教育背景"
      >
      </el-table-column>
      <el-table-column
        prop="work"
        header-align="center"
        align="center"
        label="工作经验"
      >
      </el-table-column>
      <el-table-column
        prop="description"
        header-align="center"
        align="center"
        label="个人描述"
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
            @click="goTo()"
            >修改</el-button
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
        name: '',
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
        url: this.$http.adornUrl("/user/getPersonalInfo"),
        method: "get"
      }).then(({ data }) => {
        if (data && data.code === 0) {
          this.dataList.push(data);
          console.log(this.dataList)
        } else {
          this.$message.error(data.msg)
        }
      });
    },
    goTo() {
      this.$router.push('/user-resume');
    }
  },
};
</script>
