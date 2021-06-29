<template>
  <div>
      <el-table
        :data="tableData"
        stripe
        style="width: 100%">
        <el-table-column
          prop="id"
          label="编号"
          v-if="show">
        </el-table-column>
        <el-table-column
          prop="productId"
          label="产品编号"
          width="180">
        </el-table-column>
        <el-table-column
          prop="productName"
          label="产品名称">
        </el-table-column>
        <el-table-column
          prop="type"
          label="产品档次"
          :formatter="productName">
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
          prop="type"
          label="用途类型"
          :formatter="typeName">
        </el-table-column>
        <el-table-column
          prop="register"
          label="产品经理">
        </el-table-column>
        <el-table-column label="删除">
          <template slot-scope="scope">
            <el-button type="text" @click="delete1(scope.row.id)">删除档案</el-button>
          </template>
        </el-table-column>
      </el-table>
  </div>
</template>

<script>
    export default {
        name: "Perdelete.vue",
      data() {
        return {
          show:false,
          tableData:[],
          types:[{'value':'1','lable':'商品'},
            {'value':'2','lable':'物料'}],
          productclass:[{'value':'1','lable':'高档'},
            {'value':'2','lable':'中档'},
            {'value':'3','lable':'低档'}]
        };
      },
      methods: {
        onSubmit(){
          this.$axios.post("querydelete3").then(response=>{
            this.tableData = response.data;
          }).catch();
        },
        delete1(id){
          this.$confirm('此操作将删除该商品档案, 是否继续?', '提示', {
            confirmButtonText: '确定',
            cancelButtonText: '取消',
            type: 'warning',
            center: true
          }).then(() => {
            var params = new URLSearchParams();
            params.append("id",id);
            this.$axios.post("delete",params).then(()=>{
              this.$message({
                type: 'success',
                message: '删除成功!'
              })
              this.onSubmit();
            }).catch(()=>{
              this.$message({
                type: 'danger',
                message: '删除失败'
              })
            })
          }).catch(()=>{
              this.$message({
                type: 'info',
                message: '已取消删除'
              });
          });
        },
        typeName(row){
          if(row.type=="1"){
            return "商品";
          }else if(row.type=="2"){
            return "物料";
          }else{
            return "";
          }
        },
        productName(row){
          if(row.productClass=="1"){
            return "高档";
          }else if(row.productClass=="2"){
            return "中档";
          }else if(row.productClass=="3"){
            return "低档";
          }else{
            return "";
          }
        }
      },
      created() {
        this.onSubmit();
      }
    }
</script>

<style scoped>

</style>
