<template>
    <div id="ApplySelect">
      <h4>生产计划查询</h4>
      <el-form :inline="true" label-width="100px" class="demo-ruleForm">
        <el-form-item label="生产计划编号">
          <el-input clearable v-model="applyId"></el-input>
        </el-form-item>
        <el-form-item label="产品名称">
          <el-input clearable v-model="productName"></el-input>
        </el-form-item>
        <el-form-item label="审核状态">
          <el-select clearable v-model="checkTag" placeholder="请选择审核状态">
            <el-option label="请选择" value=""></el-option>
            <el-option label="等待审核" value="s001-0"></el-option>
            <el-option label="审核通过" value="s001-1"></el-option>
            <el-option label="审核不通过" value="s001-2"></el-option>
          </el-select>
        </el-form-item>
        <el-form-item>
          <el-button type="primary" @click="submitForm">查询</el-button>
        </el-form-item>
      </el-form>
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
          width="120"
          :formatter="auditing">
        </el-table-column>
        <el-table-column
          prop="manufactureTag"
          label="派工标志"
          width="300"
          :formatter="dispatching">
        </el-table-column>
      </el-table>
    </div>
</template>

<script>
    export default {
        name: "ApplySelect",
      data(){
          return{
            tableData:[],
            applyId:"",
            productName: "",
            checkTag: ""
          }
      },
      methods:{
          getdata(){
            var _this = this;
            var params = new URLSearchParams();
            params.append("applyId",this.applyId);
            params.append("productName",this.productName);
            params.append("checkTag",this.checkTag);

            this.$axios.post("queryBy.action",params).then(function (response){
              _this.tableData = response.data;
            }).catch();
          },
        submitForm(){
            this.getdata();
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
        }
      },
      created() {
          this.getdata();
      }
    }
</script>

<style scoped>

</style>
