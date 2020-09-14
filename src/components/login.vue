<template>
  <div class="login-container">
    <div>
      <div class="login-welcome">随手记一笔账吧</div>
      <button class="login-btn" open-type="getUserInfo" @getuserinfo="getUserInfo">授权登录</button>
    </div>
  </div>
</template>

<script>
  import config from "../config";

  export default {
    methods:{
      getUserInfo(e){
        var _this = this
        let currentUser = e.target.userInfo;
        const loginUrl = config.loginUrl
        console.log(loginUrl)
        wx.login({
          success (res) {
            console.log(res)
            if (res.code) {
              console.log(res.code)
              //发起网络请求
              wx.request({
                url: loginUrl,
                data: {
                  code: res.code
                },
                success(loginRes){
                  currentUser['openId'] = (JSON.parse(loginRes.data.msg)).openid
                  console.log(currentUser)
                  wx.setStorage({
                    key:"userinfo",
                    data:currentUser
                  })
                  _this.$emit('loginsuccess')
                }
              })
            } else {
              console.log('登录失败！' + res.errMsg)
            }
            console.log("end")
          }
        })
      }
    }
  }
</script>

<style lang="scss">
  .login-container{
    align-self: center;
    justify-content: center;
    .login-welcome{
      border: 0px;
      color: #322f3b;//龙葵紫
      background-color: #e2e1e4;
      text-align: center;
      position: relative;
      top: 20rpx;
      z-index: 99;
      width: 80%;
      margin: auto;
      font-weight: bolder;

    }
    .login-btn{
      border-radius: 4px;
      background-color: #e2e1e4;//芡食白
      color: #322f3b;
      font-weight: lighter;
      width: 50vw;
      border: 2px solid #322f3b;
    }
  }
</style>
