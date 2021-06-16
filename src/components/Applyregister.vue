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
        prop="checkTag"
        label="审核标志"
        width="120">
      </el-table-column>
      <el-table-column
        prop="manufactureTag"
        label="派工标志"
        width="300">
      </el-table-column>
    </el-table>
  </div>
</template>

<script>
  export default {
    name:"Applyregister",
    data () {
      return {
        tableData:[],
      }
    },
    methods:{
        getApplyData(){
          this.$axios.get("queryAllApply.action").then((response)=>{
            this.tableData = response.data;
          }).catch();
        }
    },
    created() {
      this.getApplyData();
    }
  }
</script>

<style scoped>

</style>
