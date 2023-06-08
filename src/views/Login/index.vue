<template>
  <div class="login-container">
    <div class="logo">
      <div class="icon"></div>

      <p>HRsaas中后台管理系统</p>
    </div>
    <div class="form">
      <h3>iHRM 人力资源管理系统</h3>
      <a-card class="login-card"> 登录表单 </a-card>
    </div>
  </div>
</template>

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
