<template>
  <div id="app">
    <!-- <index msg="Welcome to Your Vue.js App"/> -->
	<div class="navbar-top">
		<ul>
			<li><a href="">登录</a></li>
			<li><a href="">帮助</a></li>
			<li>&nbsp;|&nbsp;</li>
			<li><a href="">设置</a></li>
		</ul>
	</div>
	<div class="content">
		<div class="navbar-left" :style="{flexBasis: navWidth+'px'}">
			<leftnav msg="zzzz"></leftnav>
		</div>
		<div class="dragline" id="dragline"></div>
		<div class="main">
			<newform></newform>
		</div>
	</div>
  </div>
</template>

<script>
import leftnav from './components/leftnav.vue'
import newform from './components/newform.vue'

export default {
  data: function(){
	  return {
		  navWidth: localStorage['navWidth']?parseInt(localStorage['navWidth']):280
	  }
  },
  name: 'app',
  components: {
    leftnav,
	newform
  },
  methods: {
	// 拖动垂直导航条调整宽度，默认宽度280px;最小60px;
	resize: function() {
		let data = this;
		let resize = document.getElementById("dragline");
		resize.onmousedown = function(e) {
			let startX = e.clientX;
			resize.left = resize.offsetLeft;
			document.onmousemove = function(e) {
				let endX = e.clientX;
				let moveLen = endX - startX;
				startX = endX;
				if (data.navWidth>60) {
					data.navWidth += moveLen; 
				} else if(moveLen>0){
					data.navWidth += moveLen;
				}
			};
			document.onmouseup = function() {
				document.onmousemove = null;
				document.onmouseup = null;
				localStorage['navWidth'] = data.navWidth; 
			};
			return false;
		};
	}
  },
  mounted: function(){
	this.resize();
  }
}
</script>

<style>
/* 挂载点 */
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  background-color: aliceblue;
  display: flex;
  flex-direction: column;
  height: 100%;
}

/* 顶部导航 */
.navbar-top ul {
	display: flex;
	justify-content: flex-end;
	height: 30px;
	align-items: center;
}
.navbar-top li:first-child {
	margin-right: auto;
}
/* 主体部分，共通（垂直导航+详情） */
.content {
	display: flex;
	flex: 1;
	justify-content: flex-start;
	background-color: beige;
}
/* 垂直导航 */
.navbar-left {
	flex-basis: 300px;
	overflow: auto;
	background-color: bisque;
}
/* 垂直拖拽线 */
.dragline {
	width: 0px;
	cursor: w-resize;
	border-right: #2C3E50 3px solid;
}
/* 详情页 */
.main {
	flex: 1;
	overflow: auto;
	background-color: #F5F5DC;
}
</style>
