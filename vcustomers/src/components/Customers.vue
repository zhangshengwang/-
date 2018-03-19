<template>
<div class="customers container">
  <Alert v-if="alert" v-bind:message="alert"></Alert>
  <h1 class="page-header">用户管理系统</h1>
  <input type="text" class="form-control" placeholder="搜索" v-model="filterInput"><br>
  <table class="table table-striped">
    <thead>
      <tr>
        <th>姓名</th>
        <th>电话</th>
        <th>邮箱</th>
        <th></th>
      </tr>
    </thead>

    <tbody>
      <tr v-for="customer in filterBy(customers,filterInput)">
        <!-- customers是整个数组，filterInput是要拿到的 -->
        <td>{{customer.name}}</td>
        <td>{{customer.phone}}</td>
        <td>{{customer.email}}</td>
        <td><router-link class="btn btn-defult" v-bind:to="'/customer/'+customer.id
        ">详情</router-link></td>
      </tr>
    </tbody>
  </table>
</div>
</template>

<script>
import Alert from "./Alert"
export default {
  name: 'customers',
  data() {
    return {
      customers: [],
      alert:"",
      filterInput:""
    }
  },
  components: {
    Alert
  },
  methods: {
    filterBy(customers,value){
      return customers.filter(function(customer){
        return customer.name.match(value);
        // filter便利，es6 match指的是寻找匹配的
      })
    },
    //自定义函数
    fetchCustomers() {
      this.$http.get("http://localhost:3000/users")
        .then(function(response) {
          // console.log(response) ;
          this.customers = response.body;
        })
    }

  },
  created() {
    //让加载前调用方法
    if (this.$route.query.alert){
      this.alert=this.$route.query.alert;
    }
    this.fetchCustomers();
  },
  updated() {
    //让加载前调用方法
    this.fetchCustomers();
  }
}
</script>

<style scoped>

</style>
