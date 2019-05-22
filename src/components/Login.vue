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
                  <Input v-model="formValidate.name" placeholder="请输入姓名">
                    <Icon type="ios-person-outline" slot="prepend"></Icon>
                  </Input>
                </Form-item>
                <Form-item prop="password">
                  <Input v-model="formValidate.password" placeholder="请输入密码">
                    <Icon type="ios-lock-outline" slot="prepend"></Icon>
                  </Input>
                </Form-item>
                <Row>
                  <Col span="12">
                  <Checkbox-group v-model="formValidate.remember">
                    <Checkbox label="记住我"></Checkbox>
                  </Checkbox-group>
                  </Col>
                  <Col span="12">
                  <a style="float:right" @click="toRegister">新用户注册</a>
                  </Col>
                </Row>
              </Form>
              <div slot="footer">
                <Button type="primary" size="large" long :loading="modal_loading" @click="handleSubmit('formValidate')">登录
                </Button>
                <small style="text-align: center;display:block" class="subText">©copyright by winson</small>
              </div>
            </Modal>
        </div>
    </div>
</template>

<script>
import Cookies from 'js-cookie'
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
              password: this.$data.formValidate.password
            }
          })
        }
      })
    },
    toRegister () {
      this.$router.push('/Hello')
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
