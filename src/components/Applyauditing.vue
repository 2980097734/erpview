<template>
  <div id="Applyregister">
    <el-table
      :data="tableData"
      border
      style="width: 100%">
      <el-table-column
        fixed
        prop="applyId"
        label="生产计划编号">
      </el-table-column>
      <el-table-column
        prop="productId"
        label="产品编号">
      </el-table-column>
      <el-table-column
        prop="productName"
        label="产品名称">
      </el-table-column>
      <el-table-column
        prop="amount"
        label="数量">
      </el-table-column>
      <el-table-column
        prop="checkTag"
        label="审核标志"
        :formatter="auditing">
      </el-table-column>
      <el-table-column
        fixed="right"
        label="操作">
        <template slot-scope="scope">
          <el-button @click="openupdatewin(scope.row.id)" type="text" size="medium">审核</el-button>
        </template>
      </el-table-column>
    </el-table>
<!--    审核-->
    <el-dialog title="审核" :visible="updatewinshow">
      <el-form label-width="80px" :modal="auditingData">
        <el-form-item label="id">
          <el-input v-model="auditingData.id" disabled></el-input>
        </el-form-item>
        <el-form-item label="计划编号">
          <el-input style="width: 200px" v-model="auditingData.applyId"></el-input>
          <span>产品编号</span>
          <el-input style="width: 200px" v-model="auditingData.productId"></el-input>
        </el-form-item>
        <el-form-item label="产品名称">
          <el-input style="width: 200px" v-model="auditingData.productName"></el-input>
          <span>产品数量</span>
          <el-input style="width: 200px;" v-model="auditingData.amount"></el-input>
        </el-form-item>
        <el-form-item label="设计人">
          <el-input style="width: 200px;" v-model="auditingData.designer"></el-input>
        </el-form-item>
        <el-form-item label="产品描述">
          <el-input v-model="auditingData.productDescribe"></el-input>
        </el-form-item>
        <el-form-item label="产品类型">
          <el-input v-model="auditingData.type"></el-input>
        </el-form-item>
        <el-form-item label="备注">
          <el-input v-model="auditingData.remark"></el-input>
        </el-form-item>
        <el-form-item label="登记人">
          <el-input style="width: 200px" v-model="auditingData.register"></el-input>
          <span>登记时间</span>
          <el-date-picker
            v-model="auditingData.registerTime"
            style="width: 200px"
            type="datetime"
            dataformatas="yyyy-MM-dd">
          </el-date-picker>
        </el-form-item>
        <el-form-item label="复核人">
          <el-input style="width: 200px" v-model="auditingData.checker"></el-input>
        </el-form-item>
        <el-form-item label="审核意见">
          <el-input v-model="auditingData.checkSuggestion"></el-input>
        </el-form-item>
        <el-form-item label="审核时间">
          <el-date-picker
            v-model="auditingData.checkTime"
            type="datetime"
            dataformatas="yyyy-MM-dd">
          </el-date-picker>
        </el-form-item>
        <el-form-item label="审核标志">
          <el-select v-model="auditingData.checkTag" placeholder="请选择">
            <el-option
              v-for="item in options"
              :key="item.value"
              :label="item.label"
              :value="item.value">
            </el-option>
          </el-select>
        </el-form-item>
        <el-form-item label="派工标志">
          <el-select v-model="auditingData.manufactureTag" placeholder="请选择">
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
        <el-button @click="updatewinshow = false">取 消</el-button>
        <el-button type="primary" @click="btnupdate">确 定</el-button>
      </div>
    </el-dialog>
  </div>
</template>

<script>
    export default {
        name: "Applyauditing",
      data(){
          return{
            tableData:[],
            auditingData:{},
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
            updatewinshow:false,
          }
      },
      methods: {
        getApplyData() {
          this.$axios.get("queryByCheckTag.action").then((response) => {
            this.tableData = response.data;
          }).catch();
        },
        openupdatewin(id){
          this.updatewinshow = true;
          var _this = this;
          var params = new URLSearchParams();
          params.append("id",id);

          this.$axios.post("queryById.action",params).then((response)=>{
            _this.auditingData = response.data;
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
        btnupdate(id){
          this.updatewinshow = false;
          var _this = this;
          var params = new URLSearchParams();
          Object.keys(this.auditingData).forEach(function (item) {
            if (_this.auditingData[item] != "" || _this.auditingData[item] != null){
              if (_this.auditingData[item] != checkTime){
                params.append(item,_this.auditingData[item]);
              }
            }
          })
          this.$axios.post("updateApply.action?id="+id,params).then(function (response) {
            if (response.data == true) {
              _this.$notify({
                title: '成功',
                message: '审核通过 ',
                type: 'success'
              });
            } else {
              _this.$notify({
                title: '失败',
                message: '审核不通过',
                type: 'danger'
              });
            }
            _this.getApplyData();
          })
        }
      },
      created() {
        this.getApplyData();
      }
    }
</script>

<style scoped>

</style>
