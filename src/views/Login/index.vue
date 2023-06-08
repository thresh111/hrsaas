<template>
  <div class="login-container">
    <div class="logo">
      <div class="icon"></div>

      <p>HRsaas中后台管理系统</p>
    </div>
    <div class="form">
      <h3>iHRM 人力资源管理系统</h3>
      <a-card class="login-card">
        <!-- 放置登录表单 -->
        <!-- autocomplete不自动填充表单 -->
        <a-form :model="loginForm" autocomplete="off" @finish="onFinish">
          <a-form-item
            name="mobile"
            :rules="[
              {
                required: true,
                message: '手机号不能为空',
                trigger: ['change', 'blur']
              },
              { pattern: /^1[3-9]\d{9}$/, message: '手机号格式不正确', trigger: ['change', 'blur'] }
            ]"
          >
            <!-- 手机号 -->
            <!-- v-model原理实现 v2-v3的变化  v-model :value-> modelValue -->
            <!-- v-model实现原理 :value  @input -->
            <a-input size="large" v-model:value="loginForm.mobile"></a-input>
          </a-form-item>
          <a-form-item
            name="password"
            :rules="[
              {
                required: true,
                message: '密码不能为空',
                trigger: ['change', 'blur']
              }
            ]"
          >
            <a-input-password size="large" v-model:value="loginForm.password"></a-input-password>
          </a-form-item>
          <a-form-item :rules="[{ validator: validatorAgree }]" name="isAgree">
            <a-checkbox v-model:checked="loginForm.isAgree">用户平台使用协议</a-checkbox>
          </a-form-item>
          <a-form-item>
            <a-button size="large" type="primary" htmlType="submit" block>登录</a-button>
          </a-form-item>
        </a-form>
      </a-card>
    </div>
  </div>
</template>
<script setup>
// 实现表单的校验
// 1. 声明响应式数据
import { reactive } from 'vue'
import { useRouter } from 'vue-router' // 引入方法
import { login } from '@/api/login'
import useToken from '@/stores/token'
const router = useRouter() // 得到一个router实例
// useRouter需要放置在最上层-初始化的关系

const loginForm = reactive({
  // 手机号 密码  是否同意
  mobile: 'xxxxxxxxx', // 这是我们的系统默认账号- 目的是让各位直接可以登录
  password: 'xxxxxx',
  isAgree: false // 纯前端数据 rules中的requird只能校验 null/undefined/""
})
// 自定义校验规则 rule是当前的规则 value是当前的值
const validatorAgree = (rule, value) => {
  // 判断value是不是true  如果不是true就报错
  // Promise是es6的一个构造函数- 等待 成功 失败  resolve 成功  reject失败
  return value ? Promise.resolve() : Promise.reject(new Error('您必须同意用户协议'))
}
const onFinish = async (values) => {
  const { updateToken } = useToken()
  // values是校验表单的数据
  const data = await login(values) // await是会等到 后面的promise resolve
  // 这里一定都是成功的代码
  // await必须和async配套使用 await是强制等待
  updateToken(data) // 更新pinia里面的token
  // 登录成功 要跳转到主页
  router.push('/')
}
</script>

<style lang="less">
.login-container {
  display: flex;
  align-items: stretch;
  height: 100vh;
  .logo {
    flex: 2.5;
    background: #7ba6f8 url(../../assets/common/logBg.png) no-repeat center / cover;
    border-top-right-radius: 60px;
    display: flex;
    flex-direction: column;
    align-items: flex-end;
    justify-content: center;
    padding: 0 100px;
    .icon {
      background: url(../../assets/common/logo.png) no-repeat 70px center / contain;
      width: 300px;
      height: 50px;
      margin-bottom: 50px;
      img {
        border-radius: 50%;
      }
    }
    p {
      color: #fff;
      font-size: 18px;
      margin-top: 20px;
      width: 300px;
      text-align: center;
    }
  }
  .form {
    flex: 2.5;
    display: flex;
    flex-direction: column;
    justify-content: center;
    padding-left: 50px;
    .ant-card {
      width: 320px;
      border: none;
      padding: 0;
    }
    h3 {
      padding-left: 30px;
      font-size: 24px;
    }
  }
}
</style>
