<template>
    <div class="login">
        <div class="bg">
            <Modal class="login-modal" v-model="loginModal" width="360" :closable="false" :mask-closable="false">
              <h2 class="tittle">
                <img class="flipy" :src="require('@/assets/logo.png')" alt="" height="50">
                <img class="cover" :src="require('@/assets/logo.png')" alt="" height="50">
              </h2>
              <Form ref="formValidate" method="POST" action="/user/login" :model="formValidate" :rules="ruleValidate">
                <Form-item prop="name">
                  <i-input index=1 v-model="formValidate.name" placeholder="请输入姓名">
                    <Icon type="ios-person-outline" slot="prepend"></Icon>
                  </i-input>
                </Form-item>
                <Form-item prop="password">
                  <i-input index=2 v-model="formValidate.password" placeholder="请输入密码">
                    <Icon type="ios-lock-outline" slot="prepend"></Icon>
                  </i-input>
                </Form-item>
                <Row :gutter="25">
                  <i-col span="12">
                  <Form-item prop="verifyCodeActual">
                    <i-input index=3 v-model="formValidate.verifyCodeActual" placeholder="请输入验证码" />
                  </Form-item>
                  </i-col>
                  <i-col span="12">
                    <img alt="点击更换" title="点击更换" @click="refresh()" v-bind:src="imageSrc" />
                  </i-col>
                </Row>
                <Row>
                  <i-col span="12">
                  <Checkbox-group v-model="formValidate.remember">
                    <Checkbox label="记住我"></Checkbox>
                  </Checkbox-group>
                  </i-col>
                  <i-col span="12">
                    <a style="float:right" @click="toRegister">新用户注册</a>
                  </i-col>
                </Row>
              </Form>
              <div slot="footer">
                <Button index=4 type="primary" size="large" long :loading="modal_loading" v-on:on-enter="enterFocus()"  @click="handleSubmit('formValidate')">登录
                </Button>
                <small style="text-align: center;display:block" class="subText">©copyright by winson</small>
              </div>
            </Modal>
        </div>
    </div>
</template>

<script>
// import Cookies from 'js-cookie'
export default {
  name: 'login',
  data () {
    return {
      loginModal: true,
      modal_loading: false,
      formValidate: {
        name: '',
        password: ''
      },
      imageSrc: '/kaptcha',
      ruleValidate: {
        name: [
          {required: true, message: '姓名不能为空', trigger: 'blur'}
        ],
        password: [
          {required: true, message: '密码错误', trigger: 'blur'}
        ]
      }
    }
  },
  methods: {
    handleSubmit (name) { // login
      this.$refs[name].validate((valid) => {
        if (valid) {
          this.$ajax({
            method: 'post',
            url: '/user/login',
            data: {
              username: this.$data.formValidate.name,
              password: this.$data.formValidate.password,
              verifyCodeActual: this.$data.formValidate.verifyCodeActual
            },
            headers: {'Content-Type': 'application/json;charset=utf-8'}
          })
        }
      })
    },
    toRegister () {
      this.$router.push('/Hello')
    },
    refresh () {
      console.log(this.imageSrc)
      this.imageSrc = '/kaptcha?' + Math.random()
    },
    enterFocus () {
      const DOM = event.target
      const nextDOM = DOM.nextElementSibling
      nextDOM.focus()
    }
  }
}
</script>
<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  .login{
    height: 100%;
    background: #EEDEB8 center content-box url('../assets/img/body-content-bg.jpg') repeat fixed;
  }
  .login .bg{
    padding: 32px;
    margin: 0 auto;
    height: 100%;
  }
  .login-modal{
    background: #e21515;
  }
  .tittle{
    margin: 10px;
    text-align: center;
    position: relative;
  }
  .flipy{
    -moz-transform:scaleY(-1);
    -webkit-transform:scaleY(-1);
    -o-transform:scaleY(-1);
    transform:scaleY(-1);
    /*IE*/
    filter:FlipV;
    position: absolute;
    left: auto;
    top: -15px;
  }
</style>
