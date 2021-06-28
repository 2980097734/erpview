<template>
    <div>
      <div>
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
            width="150">
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
            prop="checkTag"
            label="设计单状态"
            width="140"
            :formatter="designerFormatter">
          </el-table-column>
          <el-table-column
            prop="checkTag"
            label="审核状态"
            width="140"
            :formatter="checkTagFormatter">
          </el-table-column>
          <el-table-column
            label="装配树"
            width="140">
            <template slot-scope="scope">
              <el-button
                size="mini"
                @click="handleEdit(scope.row.id)">装配树</el-button>
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
      <el-dialog title="物料" :visible.sync="dialogTableVisible" style="width:100%">
        <el-table :data="ModuleDetails">
          <el-table-column
            label="物料编号"
            prop="productId"
            width="150">
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
            label="描述"
            prop="productDescribe">
          </el-table-column>
          <el-table-column
            label="数量"
            prop="amount">
          </el-table-column>
          <el-table-column
            label="单位"
            prop="amountUnit">
          </el-table-column>
          <el-table-column
            label="单价"
            prop="costPrice">
          </el-table-column>
          <el-table-column
            label="小计"
            prop="subtotal">
          </el-table-column>
        </el-table>
        <el-button-group>
          <el-button type="primary" @click="fanhui">返回</el-button>
        </el-button-group>
      </el-dialog>
    </div>
</template>

<script>
    export default {
        name: "wuliaochaxun",
      data(){
          return{
            ModuleData: [],
            pagesize: 5,
            pageno: 1,
            total: 0,
            ModuleOneData: {},
            onedata:[],
            twodata:[],
            threedata:[],
            ModuleDetails: [],
            form: {
              firstKindId: "",
              secondKindId: "",
              thirdKindId: "",
              startTime: "",
              endTime: ""
            },
            dialogTableVisible:false
          }
      },
      methods:{
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
          this.$axios.post("ProductTwo/queryAllModule2", params).then((response) => {
            this.ModuleData = response.data.records;
          }).catch();
        },
        typeFormatter(row){
          return row.type=="1"?"商品":"物料";
        },
        searchcartype() {   //条件查询
          this.getfiledata();
        },
        handleEdit(id) {
          var params = new URLSearchParams();
          params.append("id", id);
          this.$axios.post("ProductTwo/queryOneModule", params).then((response) => {
            this.ModuleOneData = response.data;
          }).catch();
          this.$axios.post("ProductTwo/queryByModule", params).then((response) => {
            this.ModuleDetails = response.data;
          });
          this.dialogTableVisible=true;
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
        fanhui(){
          this.dialogTableVisible=false;
          this.ModuleDetails=[];
        },
        designerFormatter(row){
          return row.checkTag=="1"?"完成":"等待"
        },
        checkTagFormatter(row){
          if (row.checkTag=="1"){
            return "通过";
          }else if (row.checkTag=="2"){
            return "未通过"
          }
          return "等待";
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
