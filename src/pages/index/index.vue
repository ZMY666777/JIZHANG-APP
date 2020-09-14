<template>
  <div>
    <div v-if="notlogin" class="login-area">
      <login @loginsuccess="loginSuccess"></login>
    </div>
  </div>
</template>

<script>
import login from "../../components/login"

export default {
  components:{
    login
  },

  data () {
    return {
        notlogin:true
      }
    },

  onLoad (option) {
    if (option.logout === 'true'){
      this.notlogin = true
    }
  },

  mounted(){
    if (wx.getStorageSync('userinfo')){
      console.log(wx.getStorageSync('userinfo'))
    }else {
      wx.hideTabBar()
    }

  },
  methods : {
    loginSuccess () {
      this.notlogin = false
      console.log('登陆成功')
      wx.showTabBar()
      wx.showToast({
        title: '登录成功',
        icon: 'success',
        duration: 2000
      })
    }
  }
}
</script>

<style scoped lang="scss">
  .index-title {
    height: 15vh;
    background-color: #322f3b;
    overflow: hidden;
  }
  .index-title > img {
    width: 100%;
    position: relative;
    top: -96px;
  }

  .login-area{
    height: 100vh;
    display: flex;
    justify-content: center;
    background-color: #e2e1e4;
  }
</style>
