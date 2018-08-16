<template>
  <div class="hello">
    <Input v-model="name" prefix="ios-contact" placeholder="用户名" style="width: auto"/>
    <Input v-model="pwd" suffix="ios-search" placeholder="密码" style="width: auto"/>
    <Button @click="register" type="default">注册</Button>
    <Button @click="login" type="primary">登录</Button>
  </div>
</template>

<script>
  import {Input, Button} from 'iview'
  import qs from 'qs'

  export default {
    name: 'HelloWorld',
    components: {Input, Button},
    data() {
      return {
        msg: 'Welcome to Your Vue.js App',
        name: '',
        pwd: ''
      }
    },
    created() {
    },
    methods: {
      login() {
        this.axios.post('/api/login', qs.stringify({user_name: this.name, user_password: this.pwd})).then((res) => {
          console.log(res)
          if (res.status === 200) {
            this.$router.push({path: '/User'})
          }
        })
      },
      register() {
        this.axios.post('/api/register', qs.stringify({user_name: this.name, user_password: this.pwd})).then((res) => {
          console.log(res)
          if (res.status === 200) {
            this.$Message.info(res.data)
          //  this.$router.push({path: '/User'})
          }
        })
      }
    }
  }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  h1, h2 {
    font-weight: normal;
  }

  ul {
    list-style-type: none;
    padding: 0;
  }

  li {
    display: inline-block;
    margin: 0 10px;
  }

  a {
    color: #42b983;
  }
</style>
