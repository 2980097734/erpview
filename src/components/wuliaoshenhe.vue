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
            <el-form-item label="登记时间">
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
        <el-table :data="ModuleData">
          <el-table-column
            prop="designId"
            label="设计编号"
            width="140">
          </el-table-column>
          <el-table-column
            prop="productId"
            label="产品编号"
            width="140">
          </el-table-column>
          <el-table-column
            prop="productName"
            label="产品名称 "
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
            width="180">
          </el-table-column>
          <el-table-column
            prop="costPriceSum"
            label="物料总成本"
            width="140">
          </el-table-column>
          <el-table-column
            label="审核"
            width="140">
            <template slot-scope="scope">
              <el-button
                size="mini"
                @click="handleEdit(scope.row.id)">审核</el-button>
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
          <template>
            <el-radio v-model="radio" label="1">未通过</el-radio>
            <el-radio v-model="radio" label="2">已通过</el-radio>
          </template>
          <template>
            <el-popconfirm
              @confirm="tijiao"
              title="确认提交吗？"
            >
              <el-button type="primary" slot="reference">复核</el-button>
            </el-popconfirm>
          </template>
          <el-button type="primary" @click="showqq">返回</el-button>
        </el-button-group>
        <el-form :model="ModuleOneData" status-icon label-width="180px" >
          <h3>物料组成设计单</h3>
          <!--<el-row :gutter="20" style="margin-top: 10px">
            <el-col :span="5">
              <div><strong>产品名称:</strong>{{fileOneData.productName}}</div>
            </el-col>
            <el-col :span="5">
              <div><strong>产品编号:</strong>{{fileOneData.productId}}</div>
            </el-col>
          </el-row>
          <el-row :gutter="20" style="margin-top: 10px" >
            <el-col :span="2">
              <el-form-item label="设计人">
                <el-input clearable v-model="designer"></el-input>
              </el-form-item>
            </el-col>
          </el-row>-->

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
                <div>{{ModuleOneData.designer}}</div>
              </el-form-item>
            </el-col>
            <el-col :span="10" >
              <el-form-item label="设计编号:">
                <div>{{ModuleOneData.designId}}</div>
              </el-form-item>
            </el-col>
          </el-row>


          <el-table :data="ModuleDetails"  >
            <el-table-column
              prop="detailsNumber"
              label="物料序号"
              width="120">
            </el-table-column>
            <el-table-column
              prop="productName"
              label="物料名称"
              width="135">
            </el-table-column>
            <el-table-column
              prop="productId"
              label="物料编号"
              width="135">
            </el-table-column>
            <el-table-column
              prop="type"
              label="用途类型"
              width="120"
              :formatter="typeFormatter">
            </el-table-column>
            <el-table-column
              prop="productDescribe"
              label="描述"
              width="135">
            </el-table-column>
            <el-table-column
              prop="amount"
              label="数量"
              width="120">
            </el-table-column>
            <el-table-column
              prop="amountUnit"
              label="单位"
              width="120">
            </el-table-column>
            <el-table-column
              prop="costPrice"
              label="单价"
              width="120">
            </el-table-column>
            <el-table-column
              label="小计"
              prop="subtotal"
              width="120">
            </el-table-column>
          </el-table>
          <el-row>
            <el-col :span="10">
              <el-form-item label="物料总成本:">
                <div>{{ModuleOneData.costPriceSum}}</div>
              </el-form-item>
            </el-col>
          </el-row>
          <el-row >
            <el-col :span="10">
              <el-form-item label="复核人:">
                <div>{{ModuleOneData.checker}}</div>
              </el-form-item>
            </el-col>
            <el-col :span="10">
              <el-form-item label="复核时间:">
                <div>{{ModuleOneData.checkTime}}</div>
              </el-form-item>

            </el-col>
          </el-row>
          <el-row>
            <el-col :span="20">
              <el-form-item label="设计要求">
                <el-input disabled  type="textarea" v-model="ModuleOneData.moduleDescribe"></el-input>
              </el-form-item>
            </el-col>
          </el-row>
        </el-form>
      </div>
    </div>
</template>

<script>
    export default {
      name: "wuliaoshenhe",
      data() {
        return {
          radio:"1",
          ModuleData: [],
          pagesize: 5,
          pageno: 1,
          total: 0,
          ModuleOneData: {},
          form: {
            firstKindId: "",
            secondKindId: "",
            thirdKindId: "",
            startTime: "",
            endTime: ""
          },
          show1: true,
          show2: false,
          ModuleDetails: [],
          checkTime: "",
          checker: "",
          onedata:[],
          twodata:[],
          threedata:[]
        }
      },
      methods: {
        getfiledata() {
          var _this = this;
          var params = new URLSearchParams();
          Object.keys(this.form).forEach((item) => {
            if(this.form[item]!=''){
              params.append(item,this.form[item]);
            }
          })
          params.append("pageno", this.pageno);
          params.append("pagesize", this.pagesize);
          this.$axios.post("ProductTwo/queryAllModule", params).then((response) => {
            this.ModuleData = response.data.records;
          }).catch();
        },
        showqq(){
          this.show2 = false;
          this.show1 = true;
          this.ModuleDetails=[];
          this.ModuleOneData={};
        },
        typeFormatter(row){
          return row.type=="Y001-1"?"商品":"物料";
        },
        searchcartype() {   //条件查询
          this.getfiledata();
        },
        handleEdit(id) {
          var params = new URLSearchParams();
          params.append("id", id);
          this.$axios.post("ProductTwo/queryOneModule", params).then((response) => {
            this.ModuleOneData = response.data;
            this.ModuleOneData.checker = sessionStorage.getItem("user");
            var date = new Date();
            var y = date.getFullYear();
            var m = date.getMonth()+1;
            var d = date.getDate();
            var h = date.getHours();
            var min = date.getMinutes();
            var s = date.getSeconds();
            this.ModuleOneData.checkTime=y+'-'+(m<10?('0'+m):m)+'-'+(d<10?('0'+d):d)+'  '+(h<10?('0'+h):h)+':'+(min<10?('0'+min):min)+':'+(s<10?('0'+s):s);

            this.show1 = false;
            this.show2 = true;
          }).catch();
          this.$axios.post("ProductTwo/queryByModule", params).then((response) => {
            this.ModuleDetails = response.data;
          });
      },
        handleSizeChange(val) {  //页size变更
          this.pagesize = val;
          this.pageno = 1;
          this.getfiledata();
        },
        handleCurrentChange(val) {  //页码变更
          this.pageno = val;
          this.getfiledata();
        },
        getonedt() {
          this.$axios.post("ProductTwo/queryAllCon").then((response) => {
            this.onedata = response.data;
          }).catch();
        },
        gettwodt() {
          var _this = this;
          var params = new URLSearchParams();
          params.append("pId", this.form.firstKindId);
          this.form.secondKindId = "";
          this.form.thirdKindId = "";
          this.$axios.post("ProductTwo/queryAllCon", params).then((response) => {
            this.twodata = response.data;
          })
        },
        getthreedt() {
          var _this = this;
          var params = new URLSearchParams();
          params.append('pId', this.form.secondKindId);
          this.form.thirdKindId = "";
          this.$axios.post("ProductMaterial/queryAllCon", params).then((response) => {
            this.threedata = response.data;
          })
        },
        tijiao() {
          if (this.radio=="1"){
            this.ModuleOneData.checkTag="S001-2";
          }else {
            this.ModuleOneData.checkTag="S001-1";
          }
          var params = new URLSearchParams();
          Object.keys(this.ModuleOneData).forEach((item) => {
            if (this.ModuleOneData[item] != '' && this.ModuleOneData[item] != null) {
              if (item != 'checkTime') {
                params.append(item, this.ModuleOneData[item]);
              }
            }
          })
          this.$axios.post("ProductTwo/moduleFh", params).then((response) => {
            this.$message({
              message: '复核成功',
              type: 'success'
            });
            this.getfiledata();
              this.showqq();
          })
        }
      },
      created() {
        this.getfiledata();
        this.getonedt();
      }
    }
</script>

<style scoped>

</style>
