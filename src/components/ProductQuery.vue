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
        <el-button type="primary" @click="onSubmit">查询</el-button>
        <el-button>取消</el-button>
      </el-form-item>
    </el-form>

    <!-- 编辑模态框-->
    <el-dialog title="类型编辑" :visible="editwinshow" width="60%" :append-to-body="true">
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
        <el-table-column label="详情">
          <template slot-scope="scope">
            <el-button
              @click="queryById(scope.row.id)"
              type="text"
              size="small">
              查看详情
            </el-button>
          </template>
        </el-table-column>
      </el-table>
      <div slot="footer" class="dialog-footer">
        <el-button @click="editwinshow = false">取 消</el-button>
      </div>

      <el-drawer
        title="复核"
        :visible.sync="dialog"
        :size="800"
        direction="ltr"
        custom-class="demo-drawer"
        ref="drawer"
        :append-to-body="true">
        <div class="demo-drawer__content">
          <el-form :model="ruleForm1" ref="ruleForm" label-width="100px" class="demo-ruleForm">
            <el-row :gutter="10">
              <el-col :span="8">
                <el-form-item label="产品名称" prop="productName">
                  <el-input v-model="ruleForm1.productName" :disabled="true"></el-input>
                </el-form-item>
              </el-col>
              <el-col :span="8">
                <el-form-item label="制造商" prop="factoryName">
                  <el-input v-model="ruleForm1.factoryName" :disabled="true"></el-input>
                </el-form-item>
              </el-col>
              <el-col :span="8">
                <el-form-item label="市场单价" prop="listPrice" label-width="100px">
                  <el-input v-model="ruleForm1.listPrice" :disabled="true"></el-input>
                </el-form-item>
              </el-col>
            </el-row>
            <el-row :gutter="10">
              <el-col :span="8">
                <el-form-item label="I级分类" prop="firstKindId">
                  <el-input v-model="ruleForm1.firstKindName" :disabled="true"></el-input>
                </el-form-item>
              </el-col>
              <el-col :span="8">
                <el-form-item label="II级分类" prop="secondKindId">
                  <el-input v-model="ruleForm1.secondKindName" :disabled="true"></el-input>
                </el-form-item>
              </el-col>
              <el-col :span="8">
                <el-form-item label="III级分类" prop="thirdKindId">
                  <el-input v-model="ruleForm1.thirdKindName" :disabled="true"></el-input>
                </el-form-item>
              </el-col>
            </el-row>
            <el-row :gutter="10">
              <el-col :span="8">
                <el-form-item label="产品简称" prop="productNick">
                  <el-input v-model="ruleForm1.productNick" :disabled="true"></el-input>
                </el-form-item>
              </el-col>
              <el-col :span="8">
                <el-form-item label="用途类型">
                  <span v-if="ruleForm1.type=='Y001-1'">商品</span>
                  <span v-if="ruleForm1.type=='Y001-2'">物料</span>
                </el-form-item>
              </el-col>
              <el-col :span="8">
                <el-form-item label="档次级别" prop="productClass">
                  <span v-if="ruleForm1.productClass=='D001-1'">高档</span>
                  <span v-if="ruleForm1.productClass=='D001-2'">中档</span>
                  <span v-if="ruleForm1.productClass=='D001-3'">低档</span>
                </el-form-item>
              </el-col>
            </el-row>
            <el-row :gutter="10">
              <el-col :span="8">
                <el-form-item label="计量单位" prop="personalUnit">
                  <el-input v-model="ruleForm1.personalUnit" :disabled="true"></el-input>
                </el-form-item>
              </el-col>
              <el-col :span="8">
                <el-form-item label="计量值" prop="personalValue">
                  <el-input v-model="ruleForm1.personalValue" :disabled="true"></el-input>
                </el-form-item>
              </el-col>
              <el-col :span="8">
                <el-form-item label="计划成本单价" label-width="150px" prop="costPrice">
                  <el-input v-model="ruleForm1.costPrice" :disabled="true"></el-input>
                </el-form-item>
              </el-col>
            </el-row>
            <hr/>
            <p style="background-color: aqua">辅助信息</p>
            <el-row :gutter="10">
              <el-col :span="8">
                <el-form-item label="保修期" prop="warranty">
                  <el-input v-model="ruleForm1.warranty" :disabled="true"></el-input>
                </el-form-item>
              </el-col>
              <el-col :span="8">
                <el-form-item label="替代品名称" prop="twinName">
                  <el-input v-model="ruleForm1.twinName" :disabled="true"></el-input>
                </el-form-item>
              </el-col>
              <el-col :span="8">
                <el-form-item label="替代品编号" prop="twinId">
                  <el-input v-model="ruleForm1.twinId" :disabled="true"></el-input>
                </el-form-item>
              </el-col>
            </el-row>
            <el-row :gutter="10">
              <el-col :span="8">
                <el-form-item label="生产周期（年）" label-width="150px" prop="lifecycle">
                  <el-input v-model="ruleForm1.lifecycle" :disabled="true"></el-input>
                </el-form-item>
              </el-col>
              <el-col :span="8">
                <el-form-item label="单位" prop="amountUnit">
                  <el-input v-model="ruleForm1.amountUnit" :disabled="true"></el-input>
                </el-form-item>
              </el-col>
              <el-col :span="8">
                <el-form-item label="产品经理" prop="responsiblePerson">
                  <el-input v-model="ruleForm1.responsiblePerson" :disabled="true"></el-input>
                </el-form-item>
              </el-col>
            </el-row>
            <el-row :gutter="10">
              <el-col :span="12">
                <el-form-item label="供应商集合" prop="providerGroup">
                  <el-input v-model="ruleForm1.providerGroup" :disabled="true"></el-input>
                </el-form-item>
              </el-col>
              <el-col :span="12">
                <el-form-item label="产品描述" prop="productDescribe">
                  <el-input v-model="ruleForm.productDescribe" :disabled="true"></el-input>
                </el-form-item>
              </el-col>
            </el-row>
            <el-row :gutter="10">
              <el-col :span="12">
                <el-form-item label="登记人" prop="register">
                  <el-input v-model="ruleForm1.register" :disabled="true"></el-input>
                </el-form-item>
              </el-col>
              <el-col :span="12">
                <el-form-item label="建档时间" prop="registerTime">
                  <el-input v-model="ruleForm1.registerTime" :disabled="true"></el-input>
                </el-form-item>
              </el-col>
            </el-row>
          </el-form>
          <div class="demo-drawer__footer">
            <el-button @click="dialog = false">取 消</el-button>
          </div>
        </div>
      </el-drawer>
    </el-dialog>

  </div>
</template>

<script>
    export default {
        name: "ProductQuery",
      data() {
        return {
          ruleForm: {},
          ruleForm1:{},
          show:false,
          editwinshow:false,
          dialog: false,
          loading: false,
          tableData:[],
          config:[],
          config1:[],
          config2:[],
          types:[{'value':'Y001-1','lable':'商品'},
            {'value':'Y001-2','lable':'物料'}],
          productclass:[{'value':'D001-1','lable':'高档'},
            {'value':'D001-2','lable':'中档'},
            {'value':'D001-3','lable':'低档'}]
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
          this.$axios.post("queryAl",params).then(response=>{
            this.tableData = response.data;
          }).catch();
        },
        queryById(id){
          this.dialog = true
          var params = new URLSearchParams();
          params.append("id",id)
          this.$axios.post("queryById",params).then(response=>{
            this.ruleForm1 = response.data;
          }).catch()
        },
        typeName(row){
          if(row.type=="Y001-1"){
            return "商品";
          }else if(row.type=="Y001-2"){
            return "物料";
          }else{
            return "";
          }
        },
        productName(row){
          if(row.productClass=="D001-1"){
            return "高档";
          }else if(row.productClass=="D001-2"){
            return "中档";
          }else if(row.productClass=="D001-3"){
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
