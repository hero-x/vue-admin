<template>
<div class="body">
    <div class="page-body">
      <div class="page-header">
        <el-breadcrumb>
          <el-breadcrumb-item :to="{path: '/'}">主页</el-breadcrumb-item>
          <el-breadcrumb-item >修改密码</el-breadcrumb-item>
        </el-breadcrumb>
      </div>
      <div class="box">
        <p>修改密码</p>
        <el-form :model="form" status-icon :rules="rules" ref="form" class="form">
          <el-form-item label="密码" prop="pass">
            <el-input type="password" v-model="form.pass" auto-complete="off"></el-input>
          </el-form-item>
          <el-form-item label="确认密码" prop="checkPass">
            <el-input type="password" v-model="form.checkPass" auto-complete="off"></el-input>
          </el-form-item>
          <el-form-item>
            <el-button type="primary" @click="submitForm('form')">提交</el-button>
            <el-button @click="resetForm('form')">重置</el-button>
          </el-form-item>
        </el-form>
      </div>
      <el-dialog
        title="修改成功!!!"
        :visible.sync="dialogVisible"
        width="30%">
        <span>你的新密码请牢记哦！</span>
        <span slot="footer" class="dialog-footer">
          <el-button type="primary" @click="handlr">确 定</el-button>
        </span>
      </el-dialog>
</div>
</div>
</template>
<script type="text/javascript">
import { mapGetters,mapActions } from 'vuex'
export default {
  data(){
    var checkPwd = (rule, value, callback) => {
      if (value === '') {
        callback(new Error('请再次输入密码'));
      } else if (value !== this.form.pass) {
        callback(new Error('两次输入密码不一致!'));
      } else {
        callback();
      }
    };
    return{
      form:{
        pass:'',
        checkPass:''
      },
      rules:{
        pass: [
          { required: true, message: '请输入密码', trigger: 'blur' },
          { min: 3, max: 8, message: '长度在 3 到 8 个字符', trigger: 'blur' },
        ],
        checkPass: [
          { validator: checkPwd, trigger: 'blur' }
        ],
      },
      // dialogVisible:false
    }
  },
  computed:{
    ...mapGetters({
      dialogVisible : 'editPwdStatus'
    })
  },
  methods:{
    ...mapActions(['getLoginAdmin', 'logout']),
    logoutAdmin(){
      this.$router.go('login')
      this.logout()
    },
    submitForm(formName){
      this.$refs[formName].validate((valid) => {
        if (valid) {
          this.$store.dispatch('editPwd',{pwd:this.$refs[formName].model.pass})
        } else {
          console.log('error submit!!');
          return false;
        }
      });
    },
    resetForm(formName){
      this.$refs[formName].resetFields();
    },
    handlr(){
      this.$store.dispatch('handlrPwdStatus')
      this.logoutAdmin()
      this.form.pass = ''
      this.form.checkPass = ''
    }
  }
}
</script>
<style type="text/css" scoped lang="less">
  .box{
    width: 34%;
    margin: 120px auto;
    height: 500px !important;
    font-size: 16px;
    p{
      font-weight: bold;
      font-size: 20px;
      border-bottom: 1px solid rgb(184, 181, 181);
      padding: 10px;
    }
  }
  .form{
    padding: 10px 60px 0 60px;
  }
  .clear{
    clear: both;
  }
</style>
