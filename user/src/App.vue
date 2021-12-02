<template>
  <div class="app">
    <div class="box">
      <div class="box-top">
        <i class="add" @click="addUser">点击添加用户</i>
        <div>
          <i>点击搜索</i>:<input  v-model="userName" placeholder="请输入名字按下回车搜索"/>
        </div>
      </div>

      <div v-show="show" class="box-min">
        名字:<input v-model="name"/>
        出生日期:<input v-model="birthday"/>
        性别:<input v-model="sex"/>
        <button @click="btn">确定添加</button>
      </div>


      <table class="box-user" border>
        <th>序号</th>
        <th>名字</th>
        <th>性别</th>
        <th>出生日期</th>
        <th>操作</th>
        <tr v-for="(users,idx) in arr" :key="idx">
          <td>{{ users.id }}</td>
          <td>{{ users.name }}</td>
          <td>{{ users.sex }}</td>
          <td>{{ users.data }}</td>
          <td>
            <el-button type="text" @click="edit(idx)">编辑</el-button>

            <el-dialog title="修改用户" :visible.sync="dialogFormVisible" :append-to-body="true">
              <el-form :model="form">
                <el-form-item label="姓名" :label-width="formLabelWidth">
                  <el-input v-model="form.name" autocomplete="off"></el-input>
                </el-form-item>
                <el-form-item label="性别" :label-width="formLabelWidth">
                  <el-input v-model="form.sex" autocomplete="off"></el-input>
                </el-form-item>
                <el-form-item label="出生日期" :label-width="formLabelWidth">
                  <el-input v-model="form.data" autocomplete="off"></el-input>
                </el-form-item>
              </el-form>
              <div slot="footer" class="dialog-footer">
                <el-button @click="dialogFormVisible = false">取 消</el-button>
                <el-button type="primary" @click="save()">确 定</el-button>
              </div>
            </el-dialog>
            <button class="remove" @click="remove(idx)">移除</button>
          </td>
        </tr>
      </table>
    </div>
  </div>
</template>
<script>
export default {
  name: 'App',
  data() {
    return {
      List: [
        {id: "0", name: 'ls', sex: '男', data: '2002-4-8'},
        {id: "1", name: 'ww', sex: '男', data: '2008-7-28'},
        {id: "2", name: 'zs', sex: '男', data: '2012-8-8'},
      ],
      arr: [],
      show: false,
      name: '',
      sex: '',
      birthday: '',
      idx: '',
      userName: '',
      form: {
        name: '',
        sex: '',
        data: ''
      },
      dialogFormVisible: false,
      formLabelWidth: '120px'
    }
  },
  methods: {
    addUser() {
      this.show = !this.show
    },
    btn() {
      if (this.name !== '' && this.sex !== '' && this.birthday !== '') {
        const add = {id: this.List.length + 1, name: this.name, sex: this.sex, data: this.birthday}
        this.List.push(add)
        this.show = !this.show
        this.name = ''
        this.sex = ''
        this.birthday = ''
      } else {
        alert("请补全输入内容")
        this.show = !this.show
      }
    },
    remove(id) {
      this.List.splice(id, 1)
    },
    edit(id) {
      this.form.name = this.List[id].name
      this.form.sex = this.List[id].sex
      this.form.data = this.List[id].data
      this.dialogFormVisible = true
      this.idx = id
    },
    save() {
      this.List[this.idx].name = this.form.name
      this.List[this.idx].sex = this.form.sex
      this.List[this.idx].data = this.form.data
      this.dialogFormVisible = false
    }
  },
  watch:{
    userName:{
      immediate:true,
      handler(val){
        this.arr = this.List.filter((users)=>{
          return users.name.indexOf(val) !==-1
        })
      }
    }
  },
  mounted() {
    this.arr = this.List
  }
}
</script>

<style>
.box {
  width: 600px;
  height: auto;
  outline: 1px black solid;
  position: fixed;
  left: 30%;
  top: 10%;
  background-color: #F2f6fC;
  border-radius: 1%;
}

.box-user {
  width: 80%;
  height: auto;
  margin: 100px auto auto;
  text-align: center;
}

.add {
  cursor: pointer;
  margin: 0 0 10px 10px;
}

.add:hover {
  color: #7e0d0d;
}

.box-min {
  width: 80%;
  height: 50px;
  outline: 1px solid black;
  margin: 50px auto auto;
  display: flex;
  align-items: center;
  justify-content: space-around;
}

.box-min > input {
  width: 50px;
}

.remove {
  color: red;
  background-color: #F2f6fC;
  border: none;
}
.box-top{
  width: 100%;
  display: flex;
  justify-content: space-around;
}
</style>
