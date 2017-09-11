<template>
  <div id="app">
    <!--<img src="./assets/logo.png">
    <router-view></router-view>-->
    <h1 v-html="msg"></h1>
    <input  type="text" v-model="itemNew" v-on:keyup.enter="addNew"/>
    <ul>
    	<li v-for="item in items" v-bind:class="{isStudent:item.student}" v-on:click="turnRed(item)">
    		{{item.name}}
    	</li>
    </ul>
    <!--<hello></hello>-->
  </div>
</template>

<script>
//使用ES6特性引入 localstorage 储存脚本，命名为 Storage
import Storage from './localstorage'
//引入 Hello.vue 组件，命名为 Hello
import Hello from './components/Hello'
//ES6语法，相当于 
//new Vue({})
export default {
  name: 'app',//name属性作为组件名称，全局 ID 自动作为组件的 name
  data () {
  	return {
  		msg:'Type name and mark who is student',
  		items:Storage.fetch(),//获取存在 storage 里面的键值对
  		itemNew:''
  	}
  },
  components:{
  	//在#app元素内，注册组件
  	//Hello
  },
  methods:{
  	turnRed: function (item) {
  		//逆反布尔值
  		item.student = !item.student;
  	},
  	addNew:function () {
  		this.items.push({
  			name : this.itemNew,
  			student : false
  		});
  		//清空文本栏
  		this.itemNew = null;
  	}
  },
  watch:{
  	items:{
	  	 handler: function (items) {
	      Storage.save(items);//监控li变化，将新增的值存入 sessionStorage 里
	    },										//sessionStorage 里的数据将在页面关闭后删除
	  	deep:true							//深度监视，只要 items 有一点改变就会触发回调函数handler
  	}
  }
}
</script>

<style>
.isStudent {
	color: red;
}
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
