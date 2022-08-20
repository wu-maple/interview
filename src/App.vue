<template>
  <div id="app">
    <el-container>
      <el-header>用户管理</el-header>
      <el-main>
        <div class="top">
          <el-button type="info" @click="handelUser">新建</el-button>
          <div>
            <el-input v-model="input" placeholder="按关键字搜索"></el-input>
            <el-button type="primary" @click="filterData">搜索</el-button>
          </div>
          <div class="chehui">
            <el-tooltip class="item" effect="dark" content="撤回已经删除的" placement="left-start">
              <el-button type="info" :disabled="returnBtnStatus" @click="returnBefore">撤回</el-button>
            </el-tooltip>
          </div>
        </div>
        <div class="data">
          <el-table border :data="tableData" style="width: 100%">
            <el-table-column align="center" width="55">
              <template slot-scope="scope">
                <el-checkbox v-model="scope.row.choose">
                </el-checkbox>
              </template>
            </el-table-column>
            <el-table-column prop="name" label="姓名" width="120">
            </el-table-column>
            <el-table-column prop="age" label="年龄" width="120">
            </el-table-column>
            <el-table-column prop="sex" label="性别" width="120">
            </el-table-column>
            <el-table-column prop="phone" label="联系电话" width="180">
            </el-table-column>
            <el-table-column prop="address" label="详细地址">
            </el-table-column>
            <el-table-column label="操作" width="160">
              <template slot-scope="scope">
                <el-button type="primary" size="mini" @click="handelEdit(scope.row.id)">编辑</el-button>
                <el-button type="danger  " size="mini" @click="handerDelete(scope.row.id)">删除</el-button>
              </template>
            </el-table-column>
          </el-table>
        </div>
        <div class="bottom">
          <el-button type="info" @click="handelMutipDelete" :disabled="btnStatus">批量删除</el-button>
        </div>
      </el-main>

      <!--  弹出框 -->
      <el-dialog :title="status == '' ? '新建用户' : '编辑用户'" :append-to-body="true" :visible.sync="dialogVisible"
        width="50%" custom-class="height">
        <el-form ref="form" :model="form" label-width="80px">
          <el-row>
            <el-col :span="12">
              <el-form-item label="姓名">
                <el-input v-model="form.name"></el-input>
              </el-form-item>
            </el-col>
            <el-col :span="12">
              <el-form-item label="性别">
                <el-select v-model="form.sex" placeholder="请选择">
                  <el-option label="男" value="男"></el-option>
                  <el-option label="女" value="女"></el-option>
                </el-select>
              </el-form-item>
            </el-col>
          </el-row>
          <el-row>
            <el-col :span="12">
              <el-form-item label="联系电话">
                <el-input v-model="form.phone"></el-input>
              </el-form-item>
            </el-col>
            <el-col :span="8">
              <el-form-item label="年龄">
                <el-input v-model.number="form.age"></el-input>
              </el-form-item>
            </el-col>
          </el-row>
          <el-row :gutter="20">
            <el-col :span="10">
              <el-form-item label="详细地址">
                <el-cascader v-model="form.chooseValue" :options="options" @change="handleChange"></el-cascader>
              </el-form-item>
            </el-col>
            <el-col :span="14">
              <el-input v-model="form.address"></el-input>
            </el-col>
          </el-row>
        </el-form>
        <span slot="footer" class="dialog-footer">
          <el-button @click="dialogVisible = false">取 消</el-button>
          <el-button type="primary" @click="handelSubmit()">保 存</el-button>
        </span>
      </el-dialog>


    </el-container>
  </div>
</template>

<script>

export default {
  name: 'App',
  computed: {
    btnStatus() {
      return this.tableData.filter(item => {
        return item.choose
      }).map(item => {
        return item.id
      }).length == 0
    },
    returnBtnStatus() {
      return this.returnArr.length == 0
    }
  },
  created() {

    if (localStorage.getItem('user') == null) {
      localStorage.setItem('user', JSON.stringify(this.tableData))
      this.tableData = JSON.parse(localStorage.getItem('user'))
    }
    else {
      this.tableData = JSON.parse(localStorage.getItem('user'))
    }

  },
  data() {
    return {
      input: "",
      form: {
        id: '',
        sex: "",
        name: "",
        phone: "",
        address: "",
        age: "",
        chooseValue: [],
      },
      status: "",
      returnArr: [],
      dialogVisible: false,
      tableData: [{
        id: 1,
        name: '王小虎',
        age: "18",
        sex: '男',
        phone: '18042463759',
        address: '上海市普陀区金沙江路 1518 弄',
        chooseValue: [],
        choose: false
      },
      {
        id: 2,
        name: 'wjw',
        age: "18",
        sex: '男',
        phone: '18042463759',
        address: '上海市普陀区金沙江路 1518 弄',
        chooseValue: [],
        choose: false
      },
      {
        id: 3,
        name: 'wwwww',
        age: "18",
        sex: '男',
        phone: '18042463759',
        address: '上海市普陀区金沙江路 1518 弄',
        chooseValue: [],
        choose: false
      }],
      options: [{
        value: '广东市',
        label: '广东市',
        children: [{
          value: '广州市',
          label: '广州市',
          children: [{
            value: '天河区',
            label: '天河区'
          }, {
            value: '番禺区',
            label: '番禺区'
          }, {
            value: '越秀区',
            label: '越秀区'
          }, {
            value: '海珠区',
            label: '海珠区'
          }, {
            value: '白云区',
            label: '白云区'
          }]
        }]
      }]
    }
  },
  methods: {
    handelUser() {
      this.dialogVisible = true;
      this.$nextTick(() => {
        this.form = {
          id: '',
          sex: "",
          name: "",
          phone: "",
          address: "",
          age: "",
          chooseValue: [],
        }
      })
    },
    handelEdit(id) {
      this.dialogVisible = true;
      this.status = "编辑";
      let data = this.tableData.filter(item => {
        return item.id == id
      })
      this.form = JSON.parse(JSON.stringify(data[0]))
    },
    handelSubmit() {
      switch (this.status) {
        case "":
          let info = {
            ...this.form,
            choose: false,
            address: this.form.chooseValue.join('-') + this.form.address,
            id: this.tableData.length + 2
          }
          this.tableData.push(info)
          localStorage.setItem('user', JSON.stringify(this.tableData))
          break;
        case "编辑":
          let data = {
            ...this.form,
            address: this.form.chooseValue.join('-') + this.form.address,
          }
          this.tableData.forEach((item, index) => {
            if (item.id == this.form.id) {
              this.tableData.splice(index, 1, data);
            }
          })
          localStorage.setItem('user', JSON.stringify(this.tableData))
          this.$message({
            type: 'success',
            message: '修改成功!'
          });
          break;
      }
      this.dialogVisible = false;
      console.log(this.$refs.form.resetFields());

      this.status = "";
    },
    handerDelete(id) {
      let data = this.tableData.filter(item => {
        return item.id == id
      })
      this.$confirm('此操作将删除该数据, 是否继续?', '提示', {
        confirmButtonText: '确定',
        cancelButtonText: '取消',
        type: 'warning'
      }).then(() => {
        this.tableData = this.tableData.filter(item => {
          return item.id != id
        })
        localStorage.setItem('user', JSON.stringify(this.tableData))
        this.returnArr.push(data[0])
        this.$message({
          type: 'success',
          message: '删除成功!'
        });
      }).catch(() => {
        this.$message({
          type: 'info',
          message: '已取消删除'
        });
      });
    },
    handelMutipDelete() {
      let data = this.tableData.filter(item => {
        return item.choose
      })
      this.$confirm('此操作将删除该数据, 是否继续?', '提示', {
        confirmButtonText: '确定',
        cancelButtonText: '取消',
        type: 'warning'
      }).then(() => {
        this.tableData = this.tableData.filter(item => {
          return !item.choose
        })
        localStorage.setItem('user', JSON.stringify(this.tableData))
        this.returnArr = data
        this.$message({
          type: 'success',
          message: '删除成功!'
        });
      }).catch(() => {
        this.$message({
          type: 'info',
          message: '已取消删除'
        });
      });
    },
    filterData() {
      if (this.input == "") {
        this.$message({
          type: 'warning',
          message: '请输入搜索关键字!'
        });
        return
      }
      const Search_List = [];
      this.tableData.forEach((item => {
        let name = item.name;
        let age = item.age;
        let sex = item.sex;
        let phone = item.phone
        let address = item.address
        if (name.includes(this.input)) {
          if (Search_List.indexOf(item) == "-1") {
            Search_List.push(item);
          }
        }
        if (age.includes(this.input)) {
          if (Search_List.indexOf(item) == "-1") {
            Search_List.push(item);
          }
        }
        if (sex.includes(this.input)) {
          if (Search_List.indexOf(item) == "-1") {
            Search_List.push(item);
          }
        }
        if (phone.includes(this.input)) {
          if (Search_List.indexOf(item) == "-1") {
            Search_List.push(item);
          }
        }
        if (address.includes(this.input)) {
          if (Search_List.indexOf(item) == "-1") {
            Search_List.push(item);
          }
        }
        this.tableData = Search_List;
      }
      ));
    },
    returnBefore() {
      this.returnArr.forEach(item => {
        this.tableData.splice(item.id - 1, 0, item)
      })
      localStorage.setItem('user', JSON.stringify(this.tableData))
      this.returnArr = [];
    },
    handleChange(value) {
      this.chooseValue = [...value]
    },
  }
}
</script>

<style lang="scss">
.chehui {
  position: absolute;
  right: 0;
}

.el-header {
  background-color: #ececec;
  display: flex;
  justify-content: center;
  align-items: center;
  font-weight: 900;
}

.el-main {
  background-color: #fff;
  height: calc(100vh - 60px);
}

.top {
  position: relative;
  display: flex;
  justify-content: flex-start;
}

.el-input {
  width: 240px !important;
}

.top .el-input__inner {
  margin-left: 20px;
  width: 200px !important;
}

.data {
  padding: 30px 0px;
}

.el-table--border th.el-table__cell {
  background: #ececec !important;
}

.height {
  height: 325px;
}

.el-dialog__body {
  height: 140px;
}
</style>
