<template>
  <div id="Applyregister">
    <el-button type="primary" @click="addwinshow=true">添加新发生生产计划</el-button>
    <el-table
      :data="tableData"
      border
      style="width: 100%">
      <el-table-column
        fixed
        prop="applyId"
        label="生产计划编号"
        width="150">
      </el-table-column>
      <el-table-column
        prop="productId"
        label="产品编号"
        width="120">
      </el-table-column>
      <el-table-column
        prop="productName"
        label="产品名称"
        width="120">
      </el-table-column>
      <el-table-column
        prop="amount"
        label="数量"
        width="120">
      </el-table-column>
      <el-table-column
        prop="checkTag"
        label="审核标志"
        width="120"
        :formatter="auditing">
      </el-table-column>
      <el-table-column
        prop="manufactureTag"
        label="派工标志"
        width="300"
        :formatter="dispatching">
      </el-table-column>
    </el-table>

    <!--  添加动态模态框-->
    <el-dialog title="新发生生产计划添加" :visible="addwinshow">
      <el-form label-width="80px" :modal="addform">
        <el-form-item label="计划编号">
          <el-input clearable v-model="addform.applyId"></el-input>
        </el-form-item>
        <el-form-item label="产品编号">
          <el-input clearable v-model="addform.productId"></el-input>
        </el-form-item>
        <el-form-item label="产品名称">
          <el-input clearable v-model="addform.productName"></el-input>
        </el-form-item>
        <el-form-item label="产品数量">
          <el-input clearable v-model="addform.amount"></el-input>
        </el-form-item>
        <el-form-item label="审核标志">
          <el-select v-model="value" placeholder="请选择">
            <el-option
              v-for="item in options"
              :key="item.value"
              :label="item.label"
              :value="item.value">
            </el-option>
          </el-select>
        </el-form-item>
        <el-form-item label="派工标志">
          <el-select v-model="value2" placeholder="请选择">
            <el-option
              v-for="item in options2"
              :key="item.value2"
              :label="item.label2"
              :value="item.value2">
            </el-option>
          </el-select>
        </el-form-item>
      </el-form>
      <div slot="footer" class="dialog-footer">
        <el-button @click="addwinshow = false">取 消</el-button>
        <el-button type="primary" @click="btnadd">确 定</el-button>
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
        applyId:"",
        productId:"",
        productName:"",
        amount:"",
        options:[{
          value:'s001-0',
          label:'等待审核'
        },{
          value: 's001-1',
          label: '审核通过'
        },{
          value: 's001-2',
          label: '审核未通过'
        }],
        options2:[{
          value2:'p001-0',
          label2:'未派工'
        },{
          value2: 'p001-1',
          label2: '已派工'
        }],
        value:"",
        value2:"",
      }
    },
    methods:{
      getApplyData(){
          this.$axios.get("queryAllApply.action").then((response)=>{
            this.tableData = response.data;
          }).catch();
        },
      auditing(row){
          if (row.checkTag=="s001-0"){
            return "等待审核"
          }
          if (row.checkTag=="s001-1"){
            return "审核通过"
          }
          if (row.checkTag=="s001-2"){
            return "审核不通过"
          }
      },
      dispatching(row){
          if (row.manufactureTag=="p001-0"){
            return "未派工"
          }
          if (row.manufactureTag=="p001-1"){
            return "已派工"
          }
      },
      btnadd(){
        //添加按钮按下，保存到db
        var _this =this;
        //组装数据(普通数据+特殊文件)   formData  html5提供的类型
        var params = new FormData();
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
          data: {params}
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
