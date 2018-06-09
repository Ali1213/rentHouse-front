<template>
  <div id="app"> 
    <template>
    <el-table
      :data="tableData"
      style="width: 100%">
      <el-table-column
        prop="title"
        label="标题"
        width="400">
      </el-table-column>
      <el-table-column
        prop="username"
        label="发布人"
        width="100">
      </el-table-column>
      <el-table-column
        label="地址">
      <template slot-scope="scope">
        <a :href="scope.row.url">点击查看</a>
      </template>
      </el-table-column>

      <!-- <el-table-column
        prop="content"
        label="内容摘要"
        width="500">
      </el-table-column> -->
      <el-table-column
        prop="create_time"
        label="发布时间">
      </el-table-column>
    </el-table>
  </template>
  </div>
</template>

<script>
import axios from 'axios'
export default {
  data() {
    return {
      tableData: [
      ]
    };
  },
  mounted () {
    Date.prototype.Format = function (fmt) {  
    var o = {
        "M+": this.getMonth() + 1, //月份 
        "d+": this.getDate(), //日 
        "h+": this.getHours(), //小时 
        "m+": this.getMinutes(), //分 
        "s+": this.getSeconds(), //秒 
        "q+": Math.floor((this.getMonth() + 3) / 3), //季度 
        "S": this.getMilliseconds() //毫秒 
    };
    if (/(y+)/.test(fmt)) fmt = fmt.replace(RegExp.$1, (this.getFullYear() + "").substr(4 - RegExp.$1.length));
    for (var k in o)
    if (new RegExp("(" + k + ")").test(fmt)) fmt = fmt.replace(RegExp.$1, (RegExp.$1.length == 1) ? (o[k]) : (("00" + o[k]).substr(("" + o[k]).length)));
    return fmt;
    }
    console.log(111)
    axios.get('http://localhost:8000/getAll')
    .then((response) =>{
      console.log(response)
      const data = response.data.data;

      this.tableData = data.map(item => {

        return {
          title: item.title,
          url: item.url,
          content: item.content.trim().slice(0,50),
          username: item.username,
          create_time: new Date(item.create_time*1000).Format("yyyy-MM-dd hh:mm:ss"),
        }
      })
    })
    .catch(function(error) {
      console.log(error)
    });

  },
  methods: {
    startHacking() {
      this.$notify({
        title: "It works!",
        type: "success",
        message:
          "We've laid the ground work for you. It's time for you to build something epic!",
        duration: 5000
      });
    }
  }
};
</script>

<style>
#app {
  font-family: Helvetica, sans-serif;
  text-align: center;
}
</style>
