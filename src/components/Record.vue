<template>
    <div>
      <p style="background-color: aqua">主信息</p>
      <el-form :model="ruleForm" ref="ruleForm" label-width="100px" class="demo-ruleForm" :rules="dateRule">
        <el-row :gutter="10">
          <el-col :span="12">
            <el-form-item label="产品名称" prop="productName">
              <el-input v-model="ruleForm.productName"></el-input>
            </el-form-item>
          </el-col>
          <el-col :span="12">
            <el-form-item label="制造商" prop="factoryName">
              <el-input v-model="ruleForm.factoryName"></el-input>
            </el-form-item>
          </el-col>
        </el-row>
        <el-row :gutter="10">
          <el-col :span="8">
            <el-form-item label="I级分类" prop="firstKindId">
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
            <el-form-item label="II级分类" prop="secondKindId">
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
            <el-form-item label="III级分类" prop="thirdKindId">
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
            <el-form-item label="产品简称" prop="productNick">
              <el-input v-model="ruleForm.productNick"></el-input>
            </el-form-item>
          </el-col>
          <el-col :span="8">
            <el-form-item label="用途类型" prop="type">
              <el-select v-model="ruleForm.type" placeholder="请选择分类">
                <el-option v-for="ty in types"
                           :value="ty.value"
                            :label="ty.lable"></el-option>
              </el-select>
            </el-form-item>
          </el-col>
          <el-col :span="8">
            <el-form-item label="档次级别" prop="productClass">
              <el-select v-model="ruleForm.productClass" placeholder="请选择分类">
                <el-option v-for="pro in productclass"
                            :value="pro.value"
                            :label="pro.lable"></el-option>
              </el-select>
            </el-form-item>
          </el-col>
        </el-row>
        <el-row :gutter="10">
          <el-col :span="6">
            <el-form-item label="计量单位" prop="personalUnit">
              <el-input v-model="ruleForm.personalUnit"></el-input>
            </el-form-item>
          </el-col>
          <el-col :span="6">
            <el-form-item label="计量值" prop="personalValue">
              <el-input v-model="ruleForm.personalValue"></el-input>
            </el-form-item>
          </el-col>
          <el-col :span="6">
            <el-form-item label="市场单价" prop="listPrice">
              <el-input v-model="ruleForm.listPrice"></el-input>
            </el-form-item>
          </el-col>
          <el-col :span="6">
            <el-form-item label="计划成本单价" label-width="150px" prop="costPrice">
              <el-input v-model="ruleForm.costPrice"></el-input>
            </el-form-item>
          </el-col>
        </el-row>
        <hr/>
        <p style="background-color: aqua">辅助信息</p>
        <el-row :gutter="10">
          <el-col :span="8">
            <el-form-item label="保修期" prop="warranty">
              <el-input v-model="ruleForm.warranty"></el-input>
            </el-form-item>
          </el-col>
          <el-col :span="8">
            <el-form-item label="替代品名称" prop="twinName">
              <el-input v-model="ruleForm.twinName"></el-input>
            </el-form-item>
          </el-col>
          <el-col :span="8">
            <el-form-item label="替代品编号" prop="twinId">
              <el-input v-model="ruleForm.twinId"></el-input>
            </el-form-item>
          </el-col>
        </el-row>
        <el-row :gutter="10">
          <el-col :span="8">
            <el-form-item label="生产周期（年）" label-width="150px" prop="lifecycle">
              <el-input v-model="ruleForm.lifecycle"></el-input>
            </el-form-item>
          </el-col>
          <el-col :span="8">
            <el-form-item label="单位" prop="amountUnit">
              <el-input v-model="ruleForm.amountUnit"></el-input>
            </el-form-item>
          </el-col>
          <el-col :span="8">
            <el-form-item label="产品经理" prop="responsiblePerson">
              <el-input v-model="ruleForm.responsiblePerson"></el-input>
            </el-form-item>
          </el-col>
        </el-row>
        <el-row :gutter="10">
          <el-col :span="12">
            <el-form-item label="供应商集合" prop="providerGroup">
              <el-input v-model="ruleForm.providerGroup"></el-input>
            </el-form-item>
          </el-col>
          <el-col :span="12">
            <el-form-item label="产品描述" prop="productDescribe">
              <el-input v-model="ruleForm.productDescribe"></el-input>
            </el-form-item>
          </el-col>
        </el-row>
        <el-row :gutter="10">
          <el-col :span="12">
            <el-form-item label="登记人" prop="register">
              <el-input v-model="ruleForm.register"></el-input>
            </el-form-item>
          </el-col>
          <el-col :span="12">
            <el-form-item label="建档时间" prop="registerTime">
              <el-input v-model="ruleForm.registerTime"></el-input>
            </el-form-item>
          </el-col>
        </el-row>
        <el-form-item>
          <el-button type="primary" @click="submitForm('ruleForm')">立即创建</el-button>
          <el-button type="danger" @click="resetForm('ruleForm')">重置</el-button>
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
              dateRule:{
                productName:[{required:true,message:'产品名称不能为空',trigger:'blur'}],
                firstKindId:[{required: true,message:'请选择I级分类',trigger:'blur'}],
                secondKindId:[{required: true,message:'请选择II级分类',trigger:'blur'}],
                thirdKindId:[{required: true,message:'请选择III级分类',trigger:'blur'}],
                listPrice:[{required: true,message:'请填入市场单价',trigger:'blur'}],
                costPrice:[{required: true,message:'请填入计划成本单价',trigger:'blur'}]
              },
              types:[{'value':'Y001-1','lable':'商品'},
                {'value':'Y001-2','lable':'物料'}],
              productclass:[{'value':'D001-1','lable':'高档'},
                {'value':'D001-2','lable':'中档'},
                {'value':'D001-3','lable':'低档'}]
            };

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
            submitForm(ruleForm){
              this.$refs[ruleForm].validate(valid=>{
                if(!valid){
                  return false;
                }else{
                  //this.from['result'] = this.result;
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
                    Object.keys(this.ruleForm).forEach(item=>{
                      this.ruleForm[item]="";
                    })
                  }).catch();
                }
              });
            },
            resetForm(ruleForm){
             // this.$refs[ruleForm].resetFields();
              Object.keys(this.ruleForm).forEach(item=>{
               this.ruleForm[item]="";
              })
            }
          },
          created(){
            this.getdataConfig();
           // this.dateRule;
          }
        }
      </script>

<style scoped>

</style>
