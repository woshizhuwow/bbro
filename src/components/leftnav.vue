<template>
  <div id="areaTree">
      <div class="box-title">
          <a href="javascript:void(0)">任务列表<i class="fa fa-refresh" @click="freshArea"></i></a>
      </div>
      <div class="tree-box">
          <div class="zTreeDemoBackground left">
              <ul id="treeDemo" class="ztree"></ul>
          </div>
      </div>
	  <div id="rMenu">
	  	<ul>
	  		<li id="m_add" @click="addTreeNode">新建</li>
	  		<li id="m_del" @click="removeTreeNode">删除</li>
	  		<li id="m_reset" @click="resetTree">恢复</li>
	  	</ul>
	  </div>
  </div>
</template>

<script>
import "../plugins/zTree_v3/js/jquery-1.4.4.min.js";
import "../plugins/zTree_v3/js/jquery.ztree.core.min.js";
import "../plugins/zTree_v3/js/jquery.ztree.excheck.min.js";	

export default {
  name: 'leftnav',
  props: {
    msg: String
  },
  data: function(){
	  return {
		  setting:{
			  edit: {
			      enable: true
			  },
			  data:{
				  simpleData:{
					  enable: true,
					  idKey: "id",
					  pIdKey: "pId",
					  rootPId: 0
				  }
			  },
			  callback: {
			  	onRightClick: this.OnRightClick
			  }
		  },
		  zNodes:[
			  {id:1, pId:0, name: "数据抓取", open:true},
			  {id:11, pId:1, name: "58同城-青岛"},
			  {id:12, pId:1, name: "51Job-青岛"},
			  {id:2, pId:0, name: "清洗规则"},
			  {id:3, pId:0, name: "定时任务"},
			  {id:4, pId:0, name: "数据导出"},
		  ],
		  zTree: null,
		  rMenu: null,
		  addCount:1
	  }
  },
  methods: {
	  freshArea:function(){
		  $.fn.zTree.init($("#treeDemo"), this.setting, this.zNodes);
	  },
	  resetTree:function () {
		this.hideRMenu();
		$.fn.zTree.init($("#treeDemo"), this.setting, this.zNodes);
	  },
	  checkTreeNode:function (checked) {
		var nodes = this.zTree.getSelectedNodes();
		if (nodes && nodes.length>0) {
			this.zTree.checkNode(nodes[0], checked, true);
		}
		this.hideRMenu();
	},
	removeTreeNode:function () {
		this.hideRMenu();
		var nodes = this.zTree.getSelectedNodes();
		if (nodes && nodes.length>0) {
			if (nodes[0].children && nodes[0].children.length > 0) {
				var msg = "要删除的节点是父节点，如果删除将删除此文件夹下所有信息。\n\n请确认！";
				if (confirm(msg)==true){
					this.zTree.removeNode(nodes[0]);
				}
			} else {
				this.zTree.removeNode(nodes[0]);
			}
		}
	},
	addTreeNode:function () {
		this.hideRMenu();
		var newNode = { name:"Blank_" + (this.addCount++)};
		if (this.zTree.getSelectedNodes()[0]) {
			newNode.checked = this.zTree.getSelectedNodes()[0].checked;
			this.zTree.addNodes(this.zTree.getSelectedNodes()[0], newNode);
		} else {
			this.zTree.addNodes(null, newNode);
		}
	},
	onBodyMouseDown:function (event){
		if (!(event.target.id == "rMenu" || $(event.target).parents("#rMenu").length>0)) {
			this.rMenu.css({"visibility" : "hidden"});
		}
	},
	hideRMenu:function () {
		if (this.rMenu) this.rMenu.css({"visibility": "hidden"});
		$("body").unbind("mousedown", this.onBodyMouseDown);
	},
	showRMenu:function (type, x, y) {
		$("#rMenu ul").show();
		if (type=="root") {
			$("#m_del").hide();
			$("#m_check").hide();
			$("#m_unCheck").hide();
		} else {
			$("#m_del").show();
			$("#m_check").show();
			$("#m_unCheck").show();
		}

		y += document.body.scrollTop;
		x += document.body.scrollLeft;
		this.rMenu.css({"top":y+"px", "left":x+"px", "visibility":"visible"});

		$("body").bind("mousedown", this.onBodyMouseDown);
	},
	OnRightClick:function (event, treeId, treeNode) {
		if (!treeNode && event.target.tagName.toLowerCase() != "button" && $(event.target).parents("a").length == 0) {
			this.zTree.cancelSelectedNode();
			this.showRMenu("root", event.clientX, event.clientY);
		} else if (treeNode && !treeNode.noR) {
			this.zTree.selectNode(treeNode);
			this.showRMenu("node", event.clientX, event.clientY);
		}
	}

  },
  mounted(){
	  $.fn.zTree.init($("#treeDemo"), this.setting, this.zNodes);
	  this.zTree = $.fn.zTree.getZTreeObj("treeDemo");
	  this.rMenu = $("#rMenu");
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>
/* @import '../plugins/zTree_v3/css/zTreeStyle/zTreeStyle.css'; */
@import '../plugins/zTree_v3/css/metroStyle/metroStyle.css';
#areaTree{
	border:1px solid #e5e5e5;    
	margin-bottom: 2px;
	border-radius: 4px;
	overflow: hidden;
}
.box-title{
	border-radius: 3px 3px 0 0;background-color: #f5f5f5;
}
.box-title a{
	color: #2fa4e7;
	text-decoration: none;font-size:14px;    display: block;
	padding: 8px 15px;cursor: pointer;
}
.box-title .fa{
	float:right;line-height: 20px;
}

/* 导航右键菜单 */
div#rMenu {position:absolute; visibility:hidden; top:0; background-color: #555; text-align: left;padding: 1px;}
div#rMenu ul li{
	font-size: 12px;
	margin-top: 1px 0;
	padding: 0 5px;
	cursor: pointer;
	list-style: none outside none;
	background-color: #DFDFDF;
}
div#rMenu ul li:hover {
	background-color: #a6a3a7;
}
</style>
