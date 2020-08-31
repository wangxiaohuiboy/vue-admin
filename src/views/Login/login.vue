<template>
  <div id="login">
    <div class="content">
      <div class="login-wrap">
        <ul class="menu-tab">
          <li
            v-for="(item,index) in menuTab"
            :key="index"
            :class="{'current':item.current}"
            @click="toggleMenu(item)"
          >{{item.txt}}</li>
        </ul>
      </div>
      <el-form
        :model="ruleForm"
        status-icon
        :rules="rules"
        ref="ruleForm"
        size="medium"
        class="demo-ruleForm"
      >
        <el-form-item prop="username">
          <label>邮箱</label>
          <el-input type="text" v-model="ruleForm.username" autocomplete="off"></el-input>
        </el-form-item>
        <el-form-item prop="password">
          <label>密码</label>
          <el-input type="password" v-model="ruleForm.password" autocomplete="off"></el-input>
        </el-form-item>
        <el-form-item prop="checkPass" v-if="model==='register'">
          <label>重复密码</label>
          <el-input type="checkPass" v-model="ruleForm.checkPass" autocomplete="off"></el-input>
        </el-form-item>
        <el-form-item prop="code">
          <label>验证码</label>
          <el-row :gutter="20">
            <el-col :span="12">
              <el-input type="code" style="width:212px" v-model="ruleForm.code"></el-input>
            </el-col>
            <el-col :span="12">
              <el-button style="marginLeft:52px;height:36px" type="success">获取验证码</el-button>
            </el-col>
          </el-row>
        </el-form-item>
        <el-form-item>
          <el-button type="danger" @click="submitForm('ruleForm')" class="login-button">提交</el-button>
        </el-form-item>
      </el-form>
    </div>
  </div>
</template>
 
<script>
import { reactive, ref } from "@vue/composition-api";
export default {
  setup(props, {refs}) {
    var validatePass = (rule, value, callback) => {
      if (value === "") {
        callback(new Error("请输入密码"));
      } else if (ruleForm.checkPass !== "") {
        $refs.ruleForm.validateField("checkPass");
      } else {
        callback();
      }
    };
    var validatorCheckPass = (rule, value, callback) => {
      if (value === "") {
        callback(new Error("请再次输入密码"));
      } else if (value !== ruleForm.password) {
        callback(new Error("两次输入密码不一致!"));
      } else {
        callback();
      }
    };
    // 这里放置data数据，生命周期，自定义的函数
    const menuTab = reactive([
      { txt: "登录", current: true, type: "login" },
      { txt: "注册", current: false, type: "register" },
    ]);
    // 模块值
    const model = ref("login");
    //表单里的数据
    const ruleForm = reactive({
      username: "",
      password: "",
      code: "",
      checkPass: "",
    });
    // 表单规则;
    const rules = reactive({
      username: [
        { required: true, message: "请输入用户名", trigger: "blur" },
        {
          type: "email",
          message: "请输入正确的邮箱地址",
          trigger: ["blur", "change"],
        },
      ],
      password: [
        { validator: validatePass, trigger: "blur" },
        { min: 6, max: 20, message: "长度在3到20个字符", trigger: "blur" },
      ],
      checkPass: [{ validator: validatorCheckPass, trigger: "blur" }],
      code: [{ required: true, message: "请输入验证码", trigger: "blur" }],
    });
    // 声明函数;
    const toggleMenu = (data) => {
      model.value = data.type;
      menuTab.forEach((el, idnex) => {
        el.current = false;
      });
      data.current = true;
    };
    //提交事件
    const submitForm = (fromName) => {
      refs[fromName].validate((valid) => {
        if (valid) {
          alert("submit");
        } else {
          console.log("error submit");
          return false;
        }
      });
      console.log(fromName);
    };

    return {
      model,
      menuTab,
      ruleForm,
      rules,
      toggleMenu,
      submitForm,
    };
  },
};
</script>
 
<style lang = "less" scoped>
#login {
  height: 100vh;
  background: #344a5f;
  .content {
    position: fixed;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    background: rgba(255, 255, 255, 0.2);
    padding: 20px 30px 30px;
    border-radius: 10px;
    .login-wrap {
      width: 330px;
      margin: 0 auto;
      .menu-tab {
        text-align: center;
        padding: 0;
        li {
          display: inline-block;
          width: 88px;
          line-height: 36px;
          font-size: 14px;
          color: #fff;
          border-radius: 2px;
          cursor: pointer;
        }
        .current {
          background: rgba(0, 0, 0, 0.1);
        }
      }
    }
    .el-form {
      label {
        display: block;
        font-size: 14px;
        color: #fff;
        line-height: 30px;
        margin-bottom: 5px;
      }
      .el-input {
        width: 329px;
      }
      .login-button {
        width: 329px;
        margin-top: 10px;
      }
      .el-form-item {
        margin-bottom: 10px;
        .el-row {
          width: 329px;
        }
      }
    }
  }
}
</style>