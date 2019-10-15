<template>
  <div class="home">
    <el-row display="margin-top:10px">
      <el-col :span="4"><el-input v-model="name" placeholder="请输入城市名称"></el-input></el-col>
      <el-col :span="4"><el-input v-model="code" placeholder="请输入城市代码"></el-input></el-col>
      <el-button type="primary" @click="addCity(name, code)">新增城市</el-button>
    </el-row>
    <el-row>
      <el-table :data="cityList" style="width: 100%" border>
        <el-table-column prop="id" label="编号" min-width="100">
          <template scope="scope">{{ scope.row.pk }}</template>
        </el-table-column>
        <el-table-column prop="city_name" label="城市名称" min-width="100">
          <template scope="scope">{{ scope.row.fields.city_name }}</template>
        </el-table-column>
         <el-table-column prop="city_code" label="城市代码" min-width="100">
          <template scope="scope">{{ scope.row.fields.city_code }}</template>
        </el-table-column>
         <el-table-column prop="create_time" label="创建时间" min-width="100">
          <template scope="scope">{{ scope.row.fields.create_time }}</template>
        </el-table-column>
      </el-table>
    </el-row>
  </div>
</template>

<script>
// @ is an alias to /src

export default {
  name: 'home',
  data(){
    return {
      name: "",
      code: 0,
      cityList: [],
    }
  },
  methods:{
    fetchList(){
      this.$http.get("/api/city_list").then(res => {
        console.log(res.body);
          if(res.body.code == 100){
              this.$message({
                type: "success",
                showClose: true,
                message: "加载成功",
              });
            this.cityList = res.body.data;
          }else{
            this.$message({
                type: "error",
                showClose: true,
                message: "加载失败",
              });
          }
      })
    },
    addCity(name, code){
      let options = {
        params: {
          "city_name": name,
          "city_code": code
        },
        emulateJSON: true,
      };
      this.$http.get("/api/add_city", options).then(res => {
          console.log(res.body);
          if(res.body.code == 100){
            this.$message({
                type: "success",
                showClose: true,
                message: "添加成功",
              });
            this.fetchList();
          }else{
            this.$message({
                type: "error",
                showClose: true,
                message: "添加失败",
              });
          }
      })
    }
  },
  mounted() {
    this.fetchList();
  }
}
</script>
