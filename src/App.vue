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
import axios from "axios";
export default {
  data() {
    return {
      tableData: []
    };
  },
  mounted() {
    var Format = function(date, fmt) {
      var o = {
        "M+": date.getMonth() + 1, //月份
        "d+": date.getDate(), //日
        "h+": date.getHours(), //小时
        "m+": date.getMinutes(), //分
        "s+": date.getSeconds(), //秒
        "q+": Math.floor((date.getMonth() + 3) / 3), //季度
        S: date.getMilliseconds() //毫秒
      };
      if (/(y+)/.test(fmt))
        fmt = fmt.replace(
          RegExp.$1,
          (date.getFullYear() + "").substr(4 - RegExp.$1.length)
        );
      for (var k in o)
        if (new RegExp("(" + k + ")").test(fmt))
          fmt = fmt.replace(
            RegExp.$1,
            RegExp.$1.length == 1
              ? o[k]
              : ("00" + o[k]).substr(("" + o[k]).length)
          );
      return fmt;
    };
    axios.get("http://23.91.96.221:8000/getAll")
      .then(response => {
        const data = response.data.data;
        this.tableData = data.map(item => {
          return {
            title: item.title,
            url: item.url,
            content: item.content.trim().slice(0, 50),
            username: item.username,
            create_time: Format(
              new Date(item.create_time * 1000),
              "yyyy-MM-dd hh:mm:ss"
            )
          };
        });
      })
      .catch(function(error) {
        console.log(error);
      });
  }
};
</script>

<style>
#app {
  font-family: Helvetica, sans-serif;
  text-align: center;
}
</style>
