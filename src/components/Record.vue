<template>
    <div>
      <p style="background-color: aqua">主信息</p>
      <el-form :model="ruleForm" ref="ruleForm" label-width="100px" class="demo-ruleForm">
        <el-row :gutter="10">
          <el-col :span="12">
            <el-form-item label="产品名称">
              <el-input v-model="ruleForm.productName"></el-input>
            </el-form-item>
          </el-col>
          <el-col :span="12">
            <el-form-item label="制造商">
              <el-input v-model="ruleForm.factoryName"></el-input>
            </el-form-item>
          </el-col>
        </el-row>
        <el-row :gutter="10">
          <el-col :span="8">
            <el-form-item label="I级分类" prop="firstKindName">
              <el-select v-model="ruleForm.firstKindId" @change="getconfig1" placeholder="请选择分类">
                <el-option
                  v-for="items in config"
                  :key="items.kindId"
                  :label="items.kindName"
                  :value="items.kindId">
                </el-option>
              </el-select>
            </el-form-item>
          </el-col>
          <el-col :span="8">
            <el-form-item label="II级分类" prop="secondKindName">
              <el-select v-model="ruleForm.secondKindId" @change="getconfig2" placeholder="请选择分类">
    <!--            <el-option label="区域一" value="shanghai"></el-option>-->
                <el-option
                  v-for="items in config1"
                  :key="items.kindId"
                  :label="items.kindName"
                  :value="items.kindId">
                </el-option>
              </el-select>
            </el-form-item>
          </el-col>
          <el-col :span="8">
            <el-form-item label="III级分类" prop="thirdKindName">
              <el-select v-model="ruleForm.thirdKindId" placeholder="请选择分类">
    <!--            <el-option label="区域一" value="shanghai"></el-option>-->
                <el-option
                  v-for="items1 in config2"
                  :key="items1.kindId"
                  :label="items1.kindName"
                  :value="items1.kindId">
                </el-option>
              </el-select>
            </el-form-item>
          </el-col>
        </el-row>
        <el-row :gutter="10">
          <el-col :span="8">
            <el-form-item label="产品简称">
              <el-input v-model="ruleForm.productNick"></el-input>
            </el-form-item>
          </el-col>
          <el-col :span="8">
            <el-form-item label="用途类型" prop="firstKindName">
              <el-select v-model="ruleForm.type" placeholder="请选择分类">
                <el-option value="商品">商品</el-option>
                <el-option value="物料">物料</el-option>
              </el-select>
            </el-form-item>
          </el-col>
          <el-col :span="8">
            <el-form-item label="档次级别" prop="firstKindName">
              <el-select v-model="ruleForm.productClass" placeholder="请选择分类">
                <el-option value="高档">高档</el-option>
                <el-option value="中档">中档</el-option>
                <el-option value="低档">低档</el-option>
              </el-select>
            </el-form-item>
          </el-col>
        </el-row>
        <el-row :gutter="10">
          <el-col :span="6">
            <el-form-item label="计量单位">
              <el-input v-model="ruleForm.personalUnit"></el-input>
            </el-form-item>
          </el-col>
          <el-col :span="6">
            <el-form-item label="计量值">
              <el-input v-model="ruleForm.personalValue"></el-input>
            </el-form-item>
          </el-col>
          <el-col :span="6">
            <el-form-item label="市场单价">
              <el-input v-model="ruleForm.listPrice"></el-input>
            </el-form-item>
          </el-col>
          <el-col :span="6">
            <el-form-item label="计划成本单价">
              <el-input v-model="ruleForm.costPrice"></el-input>
            </el-form-item>
          </el-col>
        </el-row>
        <hr/>
        <p style="background-color: aqua">辅助信息</p>
        <el-row :gutter="10">
          <el-col :span="8">
            <el-form-item label="保修期">
              <el-input v-model="ruleForm.warranty"></el-input>
            </el-form-item>
          </el-col>
          <el-col :span="8">
            <el-form-item label="替代品名称">
              <el-input v-model="ruleForm.twinName"></el-input>
            </el-form-item>
          </el-col>
          <el-col :span="8">
            <el-form-item label="替代品编号">
              <el-input v-model="ruleForm.twinId"></el-input>
            </el-form-item>
          </el-col>
        </el-row>
        <el-row :gutter="10">
          <el-col :span="8">
            <el-form-item label="生产周期（年）" label-width="150px">
              <el-input v-model="ruleForm.lifecycle"></el-input>
            </el-form-item>
          </el-col>
          <el-col :span="8">
            <el-form-item label="单位">
              <el-input v-model="ruleForm.amountUnit"></el-input>
            </el-form-item>
          </el-col>
          <el-col :span="8">
            <el-form-item label="产品经理">
              <el-input v-model="ruleForm.responsiblePerson"></el-input>
            </el-form-item>
          </el-col>
        </el-row>
        <el-row :gutter="10">
          <el-col :span="12">
            <el-form-item label="供应商集合">
              <el-input v-model="ruleForm.providerGroup"></el-input>
            </el-form-item>
          </el-col>
          <el-col :span="12">
            <el-form-item label="产品描述">
              <el-input v-model="ruleForm.productDescribe"></el-input>
            </el-form-item>
          </el-col>
        </el-row>
        <el-row :gutter="10">
          <el-col :span="12">
            <el-form-item label="登记人">
              <el-input v-model="ruleForm.register"></el-input>
            </el-form-item>
          </el-col>
          <el-col :span="12">
            <el-form-item label="建档时间">
              <el-input v-model="ruleForm.registerTime"></el-input>
            </el-form-item>
          </el-col>
        </el-row>
        <el-form-item>
          <el-button type="primary" @click="submitForm">立即创建</el-button>
          <el-button>重置</el-button>
        </el-form-item>
      </el-form>
    </div>
</template>
      <script>
        export default {
          name:"Record.vue",
          data() {
            return {
              ruleForm: {},
              config:[],
              config1:[],
              config2:[],
              finame:""
            }
          },
          methods:{
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
            submitForm(){
              var params = new FormData();
              Object.keys(this.ruleForm).forEach(item=>{
                params.append(item,this.ruleForm[item]);
              })
              this.$axios({
                method:'post',
                url:'add',
                data:params,
                headers:{
                  'Content-Type':'multipart/form-data'
                }
              }).then(response=>{
                if (response.data == true) {
                    this.$notify({
                    title: '成功',
                    message: '添加成功',
                    type: 'success'
                  });
                } else {
                    this.$notify({
                    title: '失败',
                    message: '添加失败',
                    type: 'danger'
                  });
                }
                this.getdataConfig();
              }).catch();
            }
          },
          created() {
            this.getdataConfig();
          }
        }
      </script>

<style scoped>

</style>
