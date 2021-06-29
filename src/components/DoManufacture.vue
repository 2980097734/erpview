<template>
    <div id="DoManufacture">
      <el-table
        :data="tableData"
        style="width: 100%">
        <el-table-column
          prop="id"
          label="序号"
          width="50px">
        </el-table-column>
        <el-table-column
          prop="applyId"
          label="生产计划编号">
        </el-table-column>
        <el-table-column
          prop="productId"
          label="产品编号">
        </el-table-column>
        <el-table-column
          prop="productName"
          label="产品名称">
        </el-table-column>
        <el-table-column
          prop="amount"
          label="数量">
        </el-table-column>
        <el-table-column
          prop="checkTag"
          label="生产计划审核标志"
          :formatter="auditing">
        </el-table-column>
        <el-table-column
          prop="manufactureTag"
          label="派工标志"
          :formatter="dispatching">
        </el-table-column>
        <el-table-column
          fixed="right"
          label="操作">
          <template slot-scope="scope">
            <el-button @click="openupdatewin(scope.row.id)" type="text" size="medium">派工</el-button>
          </template>
        </el-table-column>
      </el-table>

    </div>
</template>

<script>
    export default {
        name: "DoManufacture",
      data(){
          return{
            tableData:[],
            multipleSelection:[],
          }
      },
      methods:{
          getManufactureData(){
            this.$axios.get("queryByManufactureTag.action").then((response)=>{
              this.tableData = response.data;
            }).catch();
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
        dispatching(row){
          if (row.manufactureTag=="p001-0"){
            return "未派工"
          }
          if (row.manufactureTag=="p001-1"){
            return "已派工"
          }
        },
      },
      created() {
        this.getManufactureData();
      }
    }
</script>

<style scoped>

</style>
