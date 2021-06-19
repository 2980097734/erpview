<template>
  <div>
    <el-table
      :data="tableData"
      stripe
      style="width: 100%">
      <el-table-column
        prop="productId"
        label="产品编号">
      </el-table-column>
      <el-table-column
        prop="productName"
        label="产品名称">
      </el-table-column>
      <el-table-column
        prop="type"
        label="用途类型">
      </el-table-column>
      <el-table-column
        prop="firstKindName"
        label="I级分类">
      </el-table-column>
      <el-table-column
        prop="secondKindName"
        label="II级分类">
      </el-table-column>
      <el-table-column
        prop="thirdKindName"
        label="III级分类">
      </el-table-column>
      <el-table-column
        prop="register"
        label="产品经理">
      </el-table-column>
    </el-table>
  </div>
</template>

<script>
    export default {
        name: "Recheck.vue",
      data() {
        return {
          tableData: [],
          editFrom:{}
        }
      },
      methods:{
          getcheckdata(){
            this.$axios.post("queryfile").then(response=>{
              this.tableData = response.data;
            }).catch();
          }
      },
      created() {
          this.getcheckdata();
      }
    }
</script>

<style scoped>

</style>
