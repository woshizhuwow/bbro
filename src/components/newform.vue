<template>
  <div id="newform">
	<el-breadcrumb separator="/">
	  <el-breadcrumb-item>数据抓取</el-breadcrumb-item>
	  <el-breadcrumb-item>58同城-青岛</el-breadcrumb-item>
	</el-breadcrumb>
	<el-tabs tab-position="left">
		<el-tab-pane label="用户管理">
			用户管理<br>
			用户管理<br>
			用户管理<br>
			用户管理<br>
			用户管理<br>
		</el-tab-pane>
		<el-tab-pane label="配置管理">配置管理</el-tab-pane>
		<el-tab-pane label="角色管理">角色管理</el-tab-pane>
		<el-tab-pane label="定时任务补偿">定时任务补偿</el-tab-pane>
	</el-tabs>
  </div>
</template>

<script>
import Vue from 'vue';
import Element from 'element-ui';
import 'element-ui/lib/theme-chalk/index.css';
Vue.use(Element, { size: 'small', zIndex: 3000 });

export default {
  name: 'newform',
  props: {
    msg: String
  },
  data: function(){
	  return {
	  }
  },
  methods: {

  },
  mounted(){
	  
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>
	#newform {
		height: 100%;
	}
	.el-breadcrumb {
		margin: 5px;
		background-color: #7eeae3;
	}
</style>
