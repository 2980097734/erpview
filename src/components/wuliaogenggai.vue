<template>
  <div>
    <div v-show="show1">
      <div>
        <el-form ref="form" :model="form" label-width="80px">
          <el-form-item label="I级分类">
            <el-select v-model="form.firstKindId" @change="gettwodt" placeholder="请选择I级分类">
              <el-option
                v-for="item in onedata"
                :key="item.kindId"
                :label="item.kindName"
                :value="item.kindId">
              </el-option>
            </el-select>
          </el-form-item>
          <el-form-item label="II级分类">
            <el-select v-model="form.secondKindId" @change="getthreedt"  placeholder="请选择II级分类">
              <el-option
                v-for="item in twodata"
                :key="item.kindId"
                :label="item.kindName"
                :value="item.kindId">
              </el-option>
            </el-select>
          </el-form-item>
          <el-form-item label="III级分类">
            <el-select v-model="form.thirdKindId" placeholder="请选择III级分类">
              <el-option
                v-for="item in threedata"
                :key="item.kindId"
                :label="item.kindName"
                :value="item.kindId">
              </el-option>
            </el-select>
          </el-form-item>
          <el-form-item label="建档时间">
            <div class="block">
              <el-date-picker
                v-model="form.startTime"
                type="datetime"
                placeholder="选择日期时间"
                align="right">
              </el-date-picker>
              <span>至</span>
              <el-date-picker
                v-model="form.endTime"
                type="datetime"
                placeholder="选择日期时间"
                align="right">
              </el-date-picker>
              <el-button type="success" @click="searchcartype">查询</el-button>
            </div>
          </el-form-item>
        </el-form>
      </div>
      <el-table :data="ModuleData ">
        <el-table-column
          prop="designId"
          label="设计编号"
          width="140">
        </el-table-column>
        <el-table-column
          prop="productId"
          label="产品名称"
          width="140">
        </el-table-column>
        <el-table-column
          prop="productName"
          label="产品名称"
          width="140">
        </el-table-column>
        <el-table-column
          prop="designer"
          label="设计人"
          width="140">
        </el-table-column>
        <el-table-column
          prop="registerTime"
          label="登记时间"
          width="140">
        </el-table-column>
        <el-table-column
          prop="costPriceSum"
          label="物料总成本"
          width="140">
        </el-table-column>
        <el-table-column
          label="变更"
          width="140">
          <template slot-scope="scope">
            <el-button
              size="mini"
              @click="handleEdit(scope.row.id)">变更</el-button>
          </template>
        </el-table-column>
      </el-table>
      <!-- 分页-->
      <el-pagination
        @size-change="handleSizeChange"
        @current-change="handleCurrentChange"
        :current-page="pageno"
        :page-sizes="[5, 10, 15, 20]"
        :page-size="pagesize"
        layout="total, sizes, prev, pager, next, jumper"
        :total="total">
      </el-pagination>
    </div>
    <div v-show="show2">
      <el-button-group>
        <el-button type="primary" @click="showAdd">添加物料</el-button>
        <el-button type="primary" @click="delmodule">删除物料</el-button>
        <el-button type="primary" @click="showqq">返回</el-button>
        <template>
          <el-popconfirm
            @confirm="tijiao"
            title="确定提交吗？"
          >
            <el-button type="primary" slot="reference">提交</el-button>
          </el-popconfirm>
        </template>
      </el-button-group>
      <el-form :model="ModuleOneData" status-icon label-width="180px" >
        <h3>物料组成设计单</h3>
        <el-row >
          <el-col :span="10">
            <el-form-item label="产品名称：">
              <div>{{ModuleOneData.productName}}</div>
            </el-form-item>

          </el-col>
          <el-col :span="10">
            <el-form-item label="产品编号:">
              <div>{{ModuleOneData.productId}}</div>
            </el-form-item>

          </el-col>
        </el-row>
        <el-row >
          <el-col :span="10" >
            <el-form-item label="设计人:">
              <el-input clearable v-model="ModuleOneData.designer"></el-input>
            </el-form-item>
          </el-col>
        </el-row>


        <el-table :data="ModuleDetails"  :row-class-name="rowClassName"  @selection-change="xxx">
          <el-table-column
            type="selection"
            width="55">
          </el-table-column>
          <el-table-column
            prop="productId"
            label="物料编号"
            width="140">
          </el-table-column>
          <el-table-column
            prop="productName"
            label="物料名称"
            width="140">
          </el-table-column>
          <el-table-column
            prop="type"
            label="用途类型"
            width="140"
            :formatter="typeFormatter">
          </el-table-column>
          <el-table-column
            prop="productDescribe"
            label="描述"
            width="140">
          </el-table-column>
          <el-table-column
            prop="amount"
            label="数量"
            width="140">
            <template slot-scope="scope">
              <el-input clearable v-model.number="scope.row.amount" @input="amountinput(scope.row)"></el-input>
            </template>
          </el-table-column>
          <el-table-column
            prop="amountUnit"
            label="单位"
            width="140">
          </el-table-column>
          <el-table-column
            prop="costPrice"
            label="单价"
            width="140">
          </el-table-column>
          <el-table-column
            label="小计"
            prop="subtotal"
            width="140">
          </el-table-column>
        </el-table>
        <el-row >
          <el-col :span="10">
            <el-form-item label="变更人:">
              <div>{{ModuleOneData.checker}}</div>
            </el-form-item>
          </el-col>
          <el-col :span="10">
            <el-form-item label="变更时间:">
              <div>{{ModuleOneData.changeTime}}</div>
            </el-form-item>

          </el-col>
        </el-row>
        <el-row>
          <el-col :span="20">
            <el-form-item label="设计要求">
              <el-input type="textarea" v-model="ModuleOneData.moduleDescribe"></el-input>
            </el-form-item>
          </el-col>
        </el-row>
      </el-form>
    </div>

    <el-dialog title="物料" :visible.sync="dialogTableVisible" style="width:100%">
      <el-table :data="moduleDialogData">
        <el-table-column
          label="物料编号"
          prop="productId">
        </el-table-column>
        <el-table-column
          label="物料名称"
          prop="productName">
        </el-table-column>
        <el-table-column
          label="用途类型"
          prop="type"
          :formatter="typeFormatter">
        </el-table-column>
        <el-table-column
          label="物料描述"
          prop="productDescribe">
        </el-table-column>
        <el-table-column
          label="单位"
          prop="amountUnit">
        </el-table-column>
        <el-table-column
          label="计划成本单价"
          prop="costPrice">
        </el-table-column>
        <el-table-column
          align="right">
          <template slot="header" slot-scope="scope">
            <el-input
              v-model="search"
              size="mini"
              placeholder="输入关键字搜索"/>
          </template>
          <template slot-scope="scope">
            <el-button
              size="mini"
              @click="handle(scope.row)">添加</el-button>
          </template>
        </el-table-column>
      </el-table>
    </el-dialog>
  </div>
</template>

<script>
    export default {
        name: "wuliaogenggai",
      data(){
        return{
          ModuleData:[],
          pagesize:5,
          pageno:1,
          total: 0,
          form:{
            firstKindId:"",
            secondKindId:"",
            thirdKindId:"",
            startTime:"",
            endTime:""
          },
          ModuleOneData:{},
          ModuleDetails:[],
          moduleDialogData:[],
          onedata:[],
          twodata:[],
          threedata:[],
          show1:true,
          show2:false,
          search:"",
          dialogTableVisible:false,
          delmod:[]
        }
      },
      methods:{
        xxx(val){
          this.delmod=val;
        },
        getfiledata(){
          var _this=this;
          var params = new URLSearchParams();
          /* params.append("firstKindId",this.form.firstKindId);
           params.append("secondKindId",this.form.secondKindId);
           params.append("thirdKindId",this.form.thirdKindId);
           params.append("startTime",this.form.startTime);
           params.append("endTime",this.form.endTime);*/
          Object.keys(this.form).forEach((item)=>{
            if(this.form[item]!=''){
              params.append(item,this.form[item]);
            }
          })
          params.append("pageno",this.pageno);
          params.append("pagesize",this.pagesize);
          this.$axios.post("ProductMaterial/queryAllModule1",params).then((response)=>{
            this.ModuleData=response.data.records;
          }).catch();
        },
        handleEdit(id){
          var params = new URLSearchParams();
          params.append("id",id);
          this.$axios.post("ProductTwo/queryOneModule",params).then((response)=>{
            this.ModuleOneData=response.data;
            this.ModuleOneData.checker=sessionStorage.getItem("user");
            var date = new Date();
            var y = date.getFullYear();
            var m = date.getMonth()+1;
            var d = date.getDate();
            var h = date.getHours();
            var min = date.getMinutes();
            var s = date.getSeconds();
            this.ModuleOneData.changeTime=y+'-'+(m<10?('0'+m):m)+'-'+(d<10?('0'+d):d)+'  '+(h<10?('0'+h):h)+':'+(min<10?('0'+min):min)+':'+(s<10?('0'+s):s);
            this.show1=false;
            this.show2=true;
          }).catch();
          this.$axios.post("ProductTwo/queryByModule", params).then((response) => {
            this.ModuleDetails = response.data;
          });
          },
        handle(row){
          var xx=true;
          Object.keys(this.ModuleDetails).forEach((item)=>{
            if (this.ModuleDetails[item]==row){
              xx=false;
            }
          })
          if (xx){
            this.ModuleDetails.push(row);
            this.$message({
              showClose: true,
              message: '添加成功',
              type: 'success'
            });
          }else {
            this.$message({
              showClose: true,
              message: '不能重复添加',
              type: 'error'
            });
          }
        },
        showAdd(){
          this.dialogTableVisible=true;
        },
        getModuleDialogData(){
          var _this=this;
          var params = new URLSearchParams();
          params.append("productName",this.search);
          this.$axios.post("ProductMaterial/queryAllFile2",params).then((response)=>{
            this.moduleDialogData=response.data;
          }).catch();
        },
        getonedt(){
          this.$axios.post("ProductMaterial/queryAllCon").then((response)=>{
            this.onedata=response.data;
          }).catch();
        },
        gettwodt(){
          var _this=this;
          var params = new URLSearchParams();
          params.append("pId",this.form.firstKindId);
          this.form.secondKindId="";
          this.form.thirdKindId="";
          this.$axios.post("ProductMaterial/queryAllCon",params).then((response)=>{
            this.twodata=response.data;
          })
        },
        getthreedt(){
          var _this=this;
          var params = new URLSearchParams();
          params.append('pId',this.form.secondKindId);
          this.form.thirdKindId="";
          this.$axios.post("ProductMaterial/queryAllCon",params).then((response)=>{
            this.threedata=response.data;
          })
        } ,
        handleSizeChange(val) {  //页size变更
          this.pagesize = val;
          this.pageno = 1;
          this.getfiledata();
        },
        handleCurrentChange(val) {  //页码变更

          this.pageno = val;
          this.getfiledata();
        },
        searchcartype() {   //条件查询
          this.getfiledata();
        },
        typeFormatter(row){
          return row.type=="1"?"商品":"物料";
        },
        amountinput(row){
          row.subtotal=row.amount*row.costPrice;
        },
        tijiao(){
          this.ModuleDetails.forEach((item)=>{
            item.subtotal = item.amount*item.costPrice;
            item.parentId=this.ModuleOneData.id;
          });
          var params = new URLSearchParams();
          Object.keys(this.ModuleOneData).forEach((item)=>{
            if(this.ModuleOneData[item]!='' && this.ModuleOneData[item]!=null){
              params.append(item,this.ModuleOneData[item]);
            }
          })
          this.$axios.post("ProductMaterial/updateModule",params).then((response)=>{

          })
          this.$axios.post("ProductMaterial/addModuleDetails",JSON.stringify(this.ModuleDetails),
            {headers:{"Content-Type":"application/json"}}).then((response)=>{
            this.$message({
              message: '提交成功，等待审核',
              type: 'success'
            });
            this.ModuleDetails=[];
            this.ModuleOneData={};
            this.getfiledata();
            this.showqq();
          })
        },
        showqq(){
          this.show1=true;
          this.show2=false;
          this.ModuleDetails=[];
          this.ModuleOneData={};
        },
        delmodule(){
          this.delmod.forEach((item,index)=>{
            this.ModuleDetails.forEach((val,i)=>{
              if (item.index===val.index){
                this.ModuleDetails.splice(i,1)
              }
            })
          })
        },
        rowClassName(row){
          row.row.index= row.rowIndex
        }
      },
      created() {
        this.getfiledata();
        this.getonedt();
        this.getModuleDialogData();
      }
    }
</script>

<style scoped>

</style>
