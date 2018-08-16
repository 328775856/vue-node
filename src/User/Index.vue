<template>
  <div class="index">
    <Table :columns="columns" :data="dataList"></Table>
    <Modal
      v-model="modal1"
      title="编辑"
      @on-ok="ok"
      @on-cancel="cancel">
      <Input type="text" v-model="name"/>
      <Input type="text" v-model="pwd"/>
    </Modal>
  </div>
</template>

<script>
  import {Table, Button, Input, Modal} from 'iview'
  import qs from 'qs'
  export default {
    name: "Index",
    components: {Table, Button, Input, Modal},
    data() {
      let that = this
      return {
        modal1: false,
        id: '',
        name: '',
        pwd: '',
        dataList: [],
        columns: [
          {
            title: '用户ID',
            key: 'user_id'
          },
          {
            title: '用户名',
            key: 'user_name'
          },
          {
            title: '用户密码',
            key: 'user_password'
          },
          {
            title: '操作',
            render: function (h, row) {
              return (<div><div class="edit" onClick={() => {
                that.modal1 = true
                that.name = row.row.user_name
                that.pwd = row.row.user_password
                that.id = row.row.user_id
              }}>编辑</div></div>)
            }
          }
        ]
      }
    },
    created() {
      this.axios.get('/api/getUserInfo').then((res) => {
        this.dataList = res.data
      })
    },
    methods: {
      ok() {
        this.axios.post('api/editUserInfo', qs.stringify({user_id: this.id, user_name: this.name, user_password: this.pwd}))
          .then((res) => {
            console.log(res)
            if (res.data === 'ok') {
              this.axios.get('/api/getUserInfo').then((res) => {
                this.dataList = res.data
              })
            }
          })
      },
      cancel() {

      }
    }
  }
</script>

<style scoped>

</style>
