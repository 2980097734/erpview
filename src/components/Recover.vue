<template>
  <div>
    <el-form  :inline="true" :model="ruleForm" ref="ruleForm">
      <el-form-item label="请选择I级分类" prop="firstKindId">
        <el-select v-model="ruleForm.firstKindId" clearable @change="getconfig1" placeholder="请选择分类">
          <el-option
            v-for="items in config"
            :key="items.kindId"
            :label="items.kindName"
            :value="items.kindId">
          </el-option>
        </el-select>
      </el-form-item><br/>
      <el-form-item label="请选择II级分类" prop="secondKindId">
        <el-select v-model="ruleForm.secondKindId" clearable @change="getconfig2" placeholder="请选择分类">
          <!--            <el-option label="区域一" value="shanghai"></el-option>-->
          <el-option
            v-for="items in config1"
            :key="items.kindId"
            :label="items.kindName"
            :value="items.kindId">
          </el-option>
        </el-select>
      </el-form-item><br/>
      <el-form-item label="请选择III级分类" clearable prop="thirdKindId">
        <el-select v-model="ruleForm.thirdKindId" clearable placeholder="请选择分类">
          <!--            <el-option label="区域一" value="shanghai"></el-option>-->
          <el-option
            v-for="items1 in config2"
            :key="items1.kindId"
            :label="items1.kindName"
            :value="items1.kindId">
          </el-option>
        </el-select>
      </el-form-item><br/>
      <el-form-item label="请选择产品用途类型" prop="type">
        <el-select v-model="ruleForm.type" clearable placeholder="请选择分类">
          <el-option v-for="ty in types"
                     :value="ty.value"
                     :label="ty.lable"></el-option>
        </el-select>
      </el-form-item><br/>
      <el-form-item>
        <el-button type="primary" @click="onSubmit">开始</el-button>
        <el-button>取消</el-button>
      </el-form-item>
    </el-form>

    <el-dialog title="恢复档案" :visible="editwinshow" width="60%" :append-to-body="true">
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
            <el-button type="text" @click="deletecs(scope.row.id)">恢复档案</el-button>
          </template>
        </el-table-column>
      </el-table>
      <div slot="footer" class="dialog-footer">
        <el-button @click="editwinshow = false">取 消</el-button>
      </div>
    </el-dialog>
  </div>
</template>

<script>
    export default {
        name: "Recover.vue",
      data() {
        return {
          ruleForm: {},
          ruleForm1:{},
          show:false,
          editwinshow:false,
          tableData:[],
          config:[],
          config1:[],
          config2:[],
          types:[{'value':'1','lable':'商品'},
            {'value':'2','lable':'物料'}],
          productclass:[{'value':'1','lable':'高档'},
            {'value':'2','lable':'中档'},
            {'value':'3','lable':'低档'}]
        };
      },
      methods: {
        getdataConfig(){
          this.$axios.post("queryconfig").then(response =>{
            this.config = response.data;
          }).catch();
        },
        getconfig1(){
          var temp = this.config.find((item)=>{
            return item.kindId==this.ruleForm.firstKindId;
          })
          this.config1=temp.childConfig;
        },
        getconfig2(){
          var temp = this.config1.find(item=>{
            return item.kindId==this.ruleForm.secondKindId;
          })
          this.config2 = temp.childConfig;
        },
        onSubmit(){
          this.editwinshow = true;
          var params = new URLSearchParams();
          Object.keys(this.ruleForm).forEach(item=>{
            if(this.ruleForm[item]!='' && this.ruleForm[item]!=null){
              params.append(item,this.ruleForm[item]);
            }
          })
          this.$axios.post("querydelete2",params).then(response=>{
            this.tableData = response.data;
          }).catch();
        },
        deletecs(id){
          this.$confirm('此操作将恢复该商品档案, 是否继续?', '提示', {
            confirmButtonText: '确定',
            cancelButtonText: '取消',
            type: 'warning',
            center: true
          }).then(() => {
            var params = new URLSearchParams();
            params.append("id",id);
            this.$axios.post("deletecs2",params).then(()=>{
              this.$message({
                type: 'success',
                message: '恢复成功!'
              })
              this.onSubmit();
            }).catch(()=>{
              this.$message({
                type: 'danger',
                message: '恢复失败'
              })
            })
          }).catch(()=>{
              this.$message({
                type: 'info',
                message: '已取消恢复'
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
        this.getdataConfig();
      }
    }
</script>

<style scoped>

</style>
