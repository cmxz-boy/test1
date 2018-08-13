<template>
	<div class="authorityManagement">
		权限管理界面
		<div class="model-body">
			<slot name="body">
				<div class="model-body-left">
					<div class="left-top">
						模板分类
						<div class="open">
							<img src="./img/add.png" height="38" width="38" alt="展开" @click="openLists" title="展开">	
							<img src="./img/less.png" height="38" width="38" alt="闭合" @click="closeLists" title="闭合">
						</div>
					</div>
					<div class="left-bottom">
						<ul>
							<tree @getTemplateLists="list" :addTemplateListFlag.sync="addTemplateListFlag" :items.sync="items" :editListFlag.sync="editListFlag" :treeModel="treeModel" :model="model" :model-tree="modelTree" v-for="(model,index) in modelTree" :key="index"></tree>
							<div class="addTelList" @click="addTemplateList" title="添加模板分类"></div>
						</ul>
					</div>
				</div>
			</slot>
		</div>
	</div>
</template>

<script>
	import Bus from './bus.js'
	import Tree from './tree/tree'
	export default {
		name: "authorityManagement",
		data(){
			return {
				addTemplateListFlag: false,
				editListFlag: false, 
				items: [], //存储模板名称列表数据，计算属性telNameLists存储items
				modelTree: [],  //
				treeModel: {},  //获取点击的左侧tree数据
				list:[{
					EditAble:false,
					OpenFlag: false,
					childList:[
						{
							EditAble:false,
							OpenFlag: false,
							childList:[],
							deletedFlag: false,
							label: 1,
							name: '标题1-1',
							nodeType: '1',
							id: "b80ea733779496ba81a179634ef28ae",
							pId: "2ad3fd91059246608c7f62cc6ae1d0c5",
							selectFlag: false,
						},
						{
							EditAble:false,
							OpenFlag: false,
							childList:[],
							deletedFlag: false,
							label: 1,
							name: '标题1-2',
							nodeType: '1',
							id: "641e9e34df9d4592935322d51e94422b",
							pId: "2ad3fd91059246608c7f62cc6ae1d0c5",
							selectFlag: false,
						}
					],
					deletedFlag: false,
					label: 0,
					name: '标题1',
					nodeType: '1',
					id:"2ad3fd91059246608c7f62cc6ae1d0c5",
					pId: "0",
					selectFlag: false,
					}
				]
			}
		},
		created:function(){
			console.log(this.list,'list')
			// this.getTemplateLists()
		},
		methods:{
			//递归函数
			forEvent:function(modelList,flag){
				if(modelList && modelList != []){
					for(var i=0;i<modelList.length;i++){
						modelList[i].OpenFlag = flag;
						if(modelList[i].childList && modelList[i].childList != []){
							this.forEvent(modelList[i].childList, flag)
						}
					}
				}
			},
			openLists:function(){
				this.forEvent(this.modelTree,true)
			},
			closeLists:function(){
				this.forEvent(this.modelTree,false)
			},
			//新增模板一级分类
			addTemplateList:function(){
				this.forEventSelect(this.modelTree,false)
				this.buttonFlag = false
				this.items = []
				this.templateRlists = []
				var paramList = {
					EditAble: true,
					OpenFlag: false,
					childList: [],
					deletedFlag: false,
					id: "",
					label: "0",
					name: "",
					nodeType: "",
					selectType:false,
					nodeType:"1",
				}
				this.modelTree.push(paramList)
				this.addTemplateListFlag = true
			},
			//获取左侧模板分类列表
			getTemplateLists:function(){
				var list = [{
					EditAble:false,
					OpenFlag: false,
					childList:[
						{
							EditAble:false,
							OpenFlag: false,
							childList:[],
							deletedFlag: false,
							label: 1,
							name: '标题1-1',
							nodeType: '1',
							id: "b80ea733779496ba81a179634ef28ae",
							pId: "2ad3fd91059246608c7f62cc6ae1d0c5",
							selectFlag: false,
						},
						{
							EditAble:false,
							OpenFlag: false,
							childList:[],
							deletedFlag: false,
							label: 1,
							name: '标题1-2',
							nodeType: '1',
							id: "641e9e34df9d4592935322d51e94422b",
							pId: "2ad3fd91059246608c7f62cc6ae1d0c5",
							selectFlag: false,
						}
					],
					deletedFlag: false,
					label: 0,
					name: '标题1',
					nodeType: '1',
					id:"2ad3fd91059246608c7f62cc6ae1d0c5",
					pId: "0",
					selectFlag: false,
					}
				]
				this.modelTree = list
				console.log(this.modelTree,'kkk')
			}
		},
		components: {
			Tree
		}
	}
</script>

<style scoped>
.addTelList {
	width: 100px;
	height: 50px;
	line-height: 50px;
	text-align: center;
	border: 1px solid #ccc;
	font-size: 26px;
	cursor: pointer;
}
.authorityManagement > .model-body {
	width: 100%;
	height: 100%;
	font-size: 13px;
	position: relative;
	display: flex;
}
.authorityManagement > .model-body > div {
	height: 100%;
	borer: 1px solid #cfcfcf;
}
.authorityManagement > .model-body > .model-body-left {
	width: 27%;
	height: 100%;
	border-right: 0;
	box-sizing: border-box;
	overflow: hidden;
	padding-bottom: 50px;
}
.model-body-left > .left-top {
	height: 32px;
	padding-left: 10px;
	color:  #f3fbfc;
	background-color: #3ab0c8;
	line-height: 32px;
	position: relative;
}
.model-body-left > .left-top > img {
	width: 15px;
	height: 15px;
	background-size: contain;
	padding: 0 8px;
	vertical-align: text-top;
}
.open {
	position:absolute;
	top: -4px;
	right: 0;
}
.model-body-left > .left-bottom {
	width: 100%;
	height: 100%;
	background-color: #ccc;
	overflow-y: auto;
}
</style>

<style>
</style>
