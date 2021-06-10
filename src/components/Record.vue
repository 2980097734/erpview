<template>
    <div>
      <el-form :model="ruleForm" :rules="rules" ref="ruleForm" label-width="100px" class="demo-ruleForm">
        <el-form-item label="产品名称" prop="name">
          <el-input v-model="ruleForm.productName"></el-input>
        </el-form-item>
        <el-form-item label="制造商" prop="name">
          <el-input v-model="ruleForm.factoryName"></el-input>
        </el-form-item>
        <el-form-item label="制造商" prop="region">
          <el-select v-model="ruleForm.firstKindName" placeholder="请选择活动区域">
            <el-option label="区域一" value="shanghai"></el-option>
            <el-option label="区域二" value="beijing"></el-option>
          </el-select>
        </el-form-item>
        <el-form-item label="制造商" prop="region">
          <el-select v-model="ruleForm.secondKindName" placeholder="请选择活动区域">
            <el-option label="区域一" value="shanghai"></el-option>
            <el-option label="区域二" value="beijing"></el-option>
          </el-select>
        </el-form-item>
        <el-form-item label="制造商" prop="region">
          <el-select v-model="ruleForm.thirdKindName" placeholder="请选择活动区域">
            <el-option label="区域一" value="shanghai"></el-option>
            <el-option label="区域二" value="beijing"></el-option>
          </el-select>
        </el-form-item>
        <el-form-item label="产品简称" prop="name">
          <el-input v-model="ruleForm.productNick"></el-input>
        </el-form-item>
      </el-form>
    </div>
</template>
      <script>
        export default {
          data() {
            return {
              ruleForm: {
          },
          methods: {
            submitForm(formName) {
              this.$refs[formName].validate((valid) => {
                if (valid) {
                  alert('submit!');
                } else {
                  console.log('error submit!!');
                  return false;
                }
              });
            },
            resetForm(formName) {
              this.$refs[formName].resetFields();
            }
          }
        }
      </script>

<style scoped>

</style>
