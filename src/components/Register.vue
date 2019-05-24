<template>
    <div class="register">
        <Row class="bg">
            <Modal class="register-modal" v-model="registerModal" width="600" :closable="false" :mask-closable="false">
              <h2 class="tittle">
				<Button type="Text" :loading="modal_loading" ><Icon type="chevron-left"></Icon>返回</Button>
                <img class="cover" :src="require('@/assets/registerlogo.png')" alt="" height="50">
              </h2>
              <Form ref="formValidate" method="POST" action="/user/register" :model="formValidate" :rules="ruleValidate" :label-width="90">
                <Form-item label="用户名 " prop="username">
                  <Input v-model="formValidate.username" placeholder="请输入用户名" style="width: 400px"></Input>
                </Form-item>
                <Form-item label="密码 " prop="password">
                  <Input v-model="formValidate.password" placeholder="请输入密码" style="width: 400px">
                  </Input>
                </Form-item>
				<Form-item label="确认密码 " prop="confimpassword">
                  <Input v-model="formValidate.confimpassword" placeholder="请再次输入密码" style="width: 400px">
                  </Input>
                </Form-item>
				<Form-item label="手机号码 " prop="phone">
                  <Input v-model="formValidate.phone" placeholder="请输入手机号码" style="width: 200px">
                  </Input>
                </Form-item>
				<Form-item label="出生日期 " prop="birthday">
                  <Input type="date" v-model="formValidate.birthday" placeholder="请输入出生日期" style="width: 150px">
                  </Input>
                </Form-item>
              </Form>
              <div slot="footer">
				<div style="margin: 0 auto;display: block;width: 350px">
					<Button type="success" :loading="modal_loading" @click="handleSubmit('formValidate')" style="width: 150px">注册</Button>
					<Button type="primary" :loading="modal_loading"  style="width: 150px">重置</Button>
				</div>
                <small style="text-align: center;display:block" class="subText">©copyright by HSblog</small>
              </div>
            </Modal>
        </Row>
    </div>
</template>

<script>
// import Cookies from 'js-cookie'
export default {
  name: 'register',
  data () {
    return {
      registerModal: true,
      modal_loading: false,
      formValidate: {
        username: '',
        password: ''
      },
      imageSrc: '/kaptcha',
      ruleValidate: {
        username: [
          {required: true, message: '姓名不能为空', trigger: 'blur'}
        ],
        password: [
          {required: true, message: '密码不能为空', trigger: 'blur'}
        ],
        confimpassword: [
          {required: true, message: '密码不一致', trigger: 'blur'}
        ],
        phone: [
          {required: true, message: '手机号码不能为空', trigger: 'blur'}
        ],
        birthday: [
          {required: true, message: '出生日期不能为空', trigger: 'blur'}
        ]
      }
    }
  },
  methods: {
    handleSubmit (name) { // register
      this.$refs[name].validate((valid) => {
        if (valid) {
          this.$ajax({
            method: 'post',
            url: '/user/register',
            data: {
              username: this.$data.formValidate.username,
              password: this.$data.formValidate.password,
              verifyCodeActual: this.$data.formValidate.verifyCodeActual
            }
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
    }
  }
}
</script>
<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  .register{
    height: 100%;
    background: #EEDEB8 center content-box url('../assets/img/body-content-bg.jpg');
  }
  .register .bg{
    padding: 32px;
    margin: 0 auto;
    height: 100%;
  }
  .register-modal{
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
