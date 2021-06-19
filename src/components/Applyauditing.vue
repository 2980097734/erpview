<template>
  <div id="Applyregister">
    <el-table
      :data="tableData"
      border
      style="width: 100%">
      <el-table-column
        fixed
        prop="applyId"
        label="生产计划编号"
        width="150">
      </el-table-column>
      <el-table-column
        prop="productId"
        label="产品编号"
        width="120">
      </el-table-column>
      <el-table-column
        prop="productName"
        label="产品名称"
        width="120">
      </el-table-column>
      <el-table-column
        prop="amount"
        label="数量"
        width="120">
      </el-table-column>
      <el-table-column
        prop="checkTag"
        label="审核标志"
        width="120">
      </el-table-column>
      <el-table-column
        fixed="right"
        label="操作"
        width="120">
        <template slot-scope="scope">
          <el-button @click="goaudinting(scope.row)" type="text" size="small">审核</el-button>
        </template>
      </el-table-column>
    </el-table>
  </div>
</template>

<script>
    export default {
        name: "Applyauditing",
      data(){
          return{
            tableData:[]
          }
      },
      methods: {
        getApplyData() {
          this.$axios.get("queryByCheckTag.action").then((response) => {
            this.tableData = response.data;
          }).catch();
        },
        // goaudinting(row){
        //   console.log(row);
        // }
      },
      auditing(row){
        if (row.checkTag=="s001-0"){
          return "等待审核"
        }
        if (row.checkTag=="s001-1"){
          return "审核通过"
        }
        if (row.checkTag=="s001-2"){
          return "审核不通过"
        }
      },
      created() {
        this.getApplyData();
      }
    }
</script>

<style scoped>

</style>
