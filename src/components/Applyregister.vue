<template>
  <div id="Applyregister">
    <el-button type="primary" @click="addwinshow=true">添加新发生生产计划</el-button>
    <el-form label-width="80px" :modal="addform">
      <el-form-item label="编号" hidden>
        <el-input v-model="addform.id"></el-input>
      </el-form-item>
      <el-form-item label="产品编号">
        <el-input style="width: 200px" v-model="addform.productId"></el-input>
      </el-form-item>
      <el-form-item label="产品名称">
        <el-input style="width: 200px" v-model="addform.productName"></el-input>
        <span>产品数量</span>
        <el-input style="width: 200px" v-model="addform.amount"></el-input>
      </el-form-item>
      <el-form-item label="产品类型">
        <el-input v-model="addform.type"></el-input>
      </el-form-item>
      <el-form-item label="备注">
        <el-input v-model="addform.remark"></el-input>
      </el-form-item>
      <el-form-item label="登记人">
        <el-input style="width: 200px" v-model="addform.register"></el-input>
        <span>登记时间</span>
        <el-date-picker
          v-model="addform.registerTime"
          style="width: 200px"
          type="datetime"
          dataformatas="yyyy-MM-dd">
        </el-date-picker>
      </el-form-item>
      <el-form-item label="复核人" hidden>
        <el-input style="width: 200px" v-model="addform.checker"></el-input>
      </el-form-item>
      <el-form-item label="审核意见" hidden>
        <el-input v-model="addform.checkSuggestion"></el-input>
      </el-form-item>
      <el-form-item label="审核时间" hidden>
        <el-date-picker
          v-model="addform.checkTime"
          type="datetime"
          dataformatas="yyyy-MM-dd">
        </el-date-picker>
      </el-form-item>
      <el-form-item label="审核标志" hidden>
        <el-select style="width: 200px;" v-model="addform.checkTag" placeholder="请选择">
          <el-option
            v-for="item in options"
            :key="item.value"
            :label="item.label"
            :value="item.value">
          </el-option>
        </el-select>
        <span>派工标志</span>
        <el-select style="width: 200px" v-model="addform.manufactureTag" placeholder="请选择">
          <el-option
            v-for="item in options2"
            :key="item.value2"
            :label="item.label2"
            :value="item.value2">
          </el-option>
        </el-select>
      </el-form-item>
      <!--        <el-form-item label="派工标志">-->
      <!--          -->
      <!--        </el-form-item>-->
    </el-form>

    <el-button type="primary" @click="btnadd">确 定</el-button>
    <!--  添加动态模态框-->
    <el-dialog title="新发生生产计划添加" :visible="addwinshow">
      <el-table :data="tableData">
        <el-table-column
          prop="productId"
          label="产品编号"
          width="140">
        </el-table-column>
        <el-table-column
          prop="productName"
          label="产品名称"
          width="140">
        </el-table-column>
        <el-table-column
          prop="productClass"
          label="档次级别"
          width="140">
        </el-table-column>
        <el-table-column
          prop="firstKindName"
          label="I级分类"
          width="140">
        </el-table-column>
        <el-table-column
          prop="secondKindName"
          label="II级分类"
          width="140">
        </el-table-column>
        <el-table-column
          prop="thirdKindName"
          label="III级分类"
          width="140">
        </el-table-column>
        <el-table-column
          label="用途分类"
          prop="type"
          :formatter="typeFormatter"
          width="140">
        </el-table-column>
        <el-table-column
          label="生产"
          width="140">
          <template slot-scope="scope">
            <el-button
              size="mini"
              @click="handleEdit(scope.row)">生产</el-button>
          </template>
        </el-table-column>
      </el-table>
      <div slot="footer" class="dialog-footer">
        <el-button @click="addwinshow = false">取 消</el-button>

      </div>
    </el-dialog>

  </div>
</template>

<script>
  export default {
    name:"Applyregister",
    data () {
      return {
        tableData:[],
        addwinshow:false,
        addform:{},
        id:null,
        applyId:"",
        productId:"",
        productName:"",
        productDescribe:"",
        type:"",
        amount:"",
        remark:"",
        designer:"",
        register:"",
        registerTime:"",
        checker:"",
        checkSuggestion:"",
        checkTime:"",
        checkTag: "",
        manufactureTag:"",
        options:[{
          value:'0',
          label:'等待审核'
        },{
          value: '1',
          label: '审核通过'
        },{
          value: '2',
          label: '审核未通过'
        }],
        options2:[{
          value2:'0',
          label2:'未派工'
        },{
          value2: '1',
          label2: '已派工'
        }],
        value:"",
        value2:"",
      }
    },
    methods:{
      getApplyData(){
          this.$axios.get("selectAllFile.action").then((response)=>{
            this.tableData = response.data;
          }).catch();
        },
      typeFormatter(row){
        return row.type=="1"?"商品":"物料";
      },
      handleEdit(row){
        this.addform=row;
      },
      auditing(row){
          if (row.checkTag=="0"){
            return "等待审核"
          }
          if (row.checkTag=="1"){
            return "审核通过"
          }
          if (row.checkTag=="2"){
            return "审核不通过"
          }
      },
      dispatching(row){
          if (row.manufactureTag=="0"){
            return "未派工"
          }
          if (row.manufactureTag=="1"){
            return "已派工"
          }
      },
      btnadd(){
        //添加按钮按下，保存到db
        var _this =this;
        //组装数据(普通数据+特殊文件)   formData  html5提供的类型
        var params = new FormData();
        var date = new Date();
        var y = date.getFullYear();
        var m = date.getMonth()+1;
        var d = date.getDate();
        var h = date.getHours();
        var min = date.getMinutes();
        var s = date.getSeconds();
        this.addform.registerTime =
          y+ '-'+(m<10?('0'+m):m)+'-'+(d<10?('0'+d):d)+' '+(h<10?('0'+h):h)+':'+(min<10?('0'+min):min)+':'+(s<10?('0'+s):s);
        Object.keys(this.addform).forEach((item)=>{
          params.append(item,this.addform[item]);
        })
        //异步请求    post   enctype
        /*  this.$axios.post("addCartype.action",params,{headers: {
              'Content-Type':'multipart/form-data'
            }}).then().catch();*/
        this.$axios({
          method: 'post',
          url: 'addApply.action',
          data: params
          // headers: {
          //   'Content-Type':'multipart/form-data'
          // }
        }).then(function (response) {
          if (response.data == true) {
            _this.$notify({
              title: '成功',
              message: '添加成功',
              type: 'success'
            });
          } else {
            _this.$notify({
              title: '失败',
              message: '添加失败',
              type: 'danger'
            });
          }
          //刷新表格数据
          _this.getApplyData();
          _this.addwinshow=false;
        }).catch();
      }
    },
    created() {
      this.getApplyData();
    }
  }
</script>

<style scoped>

</style>
