<template>
	<div class="tree" v-if="!modelItem.deletedFlag">
		<li>
			<div @click.stop="toggle(modelItem)" class="titleName" :class="modelItem.selectFlag ? 'tag' : ''">
				<div :style="{width:modelItem.label*10+ 'px'}" v-if="modelItem.label"></div>
				<i class="fa" v-if="isFolder" :class="modelItem.OpenFlag? 'fa-folder-open-j' : 'fa-folder-j'"></i>
				<i class="fa" v-if="!isFolder"></i>	
				<input v-focus ref="input" v-if="modelItem.EditAble" type="text" v-model="modelItem.name" @click.stop="stopBubbing" @blur="blurEvent(modelItem)">
				<span v-else :title="model.name">{{model.name}}</span>
				<div class="icon_operation" v-show="modelItem.label != 0">
					<img src="../img/mark-add.png" alt="添加" @click.stop="addTemplateChilkldList(modelItem)" title="添加" v-if="modelItem.nodeType==1">
					<img src="../img/mark-edit.png" alt="修改" @click.stop="editTemplateList(modelItem)" title="修改">
					<img src="../img/mark-del.png" alt="删除" @click.stop="deletModelList(modelItem)" title="删除">
				</div>
			</div>
			<ul v-show="modelItem.OpenFlag" v-if="isFolder">
				<tree :items.sync="items" :model="cel" v-for="(cel,index) in modelItem.childList" :key="index" :model-tree="mymodelTree"></tree>
			</ul>
		</li>
	</div>
</template>

<script>
import Bus from '../bus.js'
	export default {
		name: "tree",
		props:["model","treeModel","modelTree","items","getTemplateLists","taddTemplateListFlag"],
		data(){
			return {
				mymodelTree: this.modelTree, //tree全部数据
				clickModelList: {}, //暂存当前点击的tree列表
				userInfo: {},  //存登录人信息
				blurFlag: true,  //新建的分类是否保存
				testArr: null,
				paramList: {},  //暂存添加的子类数据
				ownerCd: "",  //存放切换的ownerCd
				getAddName: '', //添加模板列表是赋值，tree的添加按钮隐藏
				addAndDeleteFlag: "",  //添加后未保存tree列表标记
				treeFlag: ""
			}
		},
		computed:{
			//含有子节点的展开收起按钮
			isFolder:function(){
				return this.modelItem.childList && this.modelItem.childList.length
			},
			//缓存model
			modelItem:function(){
				return this.model
			}
		},
		created:function(){
			console.log(this.model,'tree:model')
		},
		mounted:function(){
			// Bus.$on("getAddNameFlag",this.getAddNameFlag)
			// Bus.$on("getTreeFlag",this.getTreeFlag)
		},
		watch: {
			treeFlag:function(){
				if(this.treeFlag == this.model.id){
					this.model.nodeType = 1
				}
			},
			getAddName: {
				handler:function(){
					if(this.getAddName == this.model.id){
						this.model.nodeType = 10
					}
				},
				deep: true
			},
			modelTree:function(val){
				this.mymodelTree = val
			},
		},
		methods:{
			//递归函数
			
			//模板分类新增赋值返回id，前端更新分类列表用以查询模板名称列表
			forListEvent:function(model,modelId){
				for(var i=0;i<model.length;i++){
					if(model[i].id == ""){
						model[i].id = modelId
					}
					if(model[i].childList &&model[i].childList != []){
						this.forListEvent(model[i].childList,modelId)
					}
				}
			},
			stopBubbing:function(){},  //点击模板名称input阻止冒泡
			blurEvent:function(modelItem){
				modelItem.EditAble = false
				if(modelItem.id && modelItem.id != ""){
					//修改name离焦保存
					if(modelItem.name && modelItem.name != ""){
						//发请求进行修改
						// this.$fetchData("put",interfaces.saveClassifyName,modelItem).then{}
						this.blurFlag = true
						console.log(this,'this')
						alert('修改成功')
					}else{
						alert('名称不能为空!',true,"tips")
						modelItem.EditAble = true
						this.$refs["input"].focus()
					}
				}else{
					//新建离焦保存
					if(modelItem.name && modelItem.name != ""){
						var params = {
							id: "",
							name: modelItem.name,
							docCd: "102",
							deletedFlag: false,
							//左侧列表id
							pId: modelItem.pId ? modelItem.pId : "0",
							nodeType: "1",
							//模板内容数据结构主宿
							EditAble:true,
							OpenFlag: false,
							childList: [],
							deletedFlag: false,
							label: modelItem.label,
							selectFlag: true
						}
						// this.$fetchData("POST",interfaces.addClassifyList+"1",params).then(res => {
						// 	if(res.code == 0){
						// 		alert("新建成功")
						// 	}
						// })
					}else{
						console.log(this.$parent.model,"parent:model")
						if(this.$parent.model && this.$parent.model.childList.length != 0){
							this.$parent.model.childList.pop()
						}else{
							//新建没有保存直接隐藏
							modelItem.deletedFlag = true
							this.$emit("update:addTemplateListFlag",false)
						}
					}					
				}
			},
			//添加
			addTemplateChilkldList:function(modelItem){
				Bus.$emit("addOrDelet")  //触发case组件事件用来监听添加删除事件
				modelItem.OpenFlag = true
				this.setSelectedFlag(this.modelTree)
				modelItem.selectFlag = false
				modelItem.EditAble = false
				this.paramList = {  
					EditAble: true,
					OpenFlag: false,
					childList: [],
					deletedFlag: false,
					id: "",
					label: parseInt(modelItem.label) + 1,
					name: "",
					nodeType: 1,
					pId: modelItem.id,
					selectFlag: false
				}
				modelItem.childList.push(this.paramList)
				this.testArr = modelItem
			},
			//编辑
			editTemplateList:function(modelItem){
				modelItem.EditAble = true;
			},
			//删除分类节点
			deletModelList:function(modelItem){
				this.addAndDeleteFlag = "falg"
				// var _this = this
				// if(modelItem &&modelItem.id){
				// 	confirm("此操作将永久删除该文件，是否删除?",function(flag){
				// 		if(flag){
				// 			if(modelItem.childList && modelItem.childList.length != 0){
				// 				alert("要删除的节点存在子节点不允许直接删除!",true,"tips")
				// 			}else{
				// 				_this.$fetchData("DELETE",interfaces.deletModelList + modelItem.id).then(res => {
				// 					if(res.code == 0){
				// 						if(_this.$parent.model && _this.$parent.model.childList && _this.$parent.model.childList != undefined){
				// 							_this.$parent.$children.forEach(function(item,index){
				// 								if(item.model.id == modelItem.id){
				// 									_this.$parent.model.childList.splice(index,1)
				// 									_this.$emit("update:addTemplateListFlag",false)
				// 									alert("删除成功",true,"success")
				// 									Bus.$emit("addOrDelet") //触发case组件事件用来监听添加删除事件
				// 								}
				// 							})
				// 						}else{

				// 						}
				// 					}else if(res.code == 1 && res.msg == "删除失败，该Id下存在子节点不允许删除"){
				// 						alert('要删除的节点存在病例模板不允许直接删除!',true,"tips")
				// 					}
				// 				})
				// 			}
				// 		}
				// 	},"tips")
				// }else{
				// 	//新建没有保存直接隐藏
				// 	modelItem.deletedFlag = true
				// 	this.$emit("update:addTemplateListFlag",false)
				// }
			},
			//tree点击事件
			toggle:function(model){
				this.clickModelList = model
				this.setSelectedFlag(this.mymodelTree)
				model.selectFlag = true
				model.OpenFlag = !model.OpenFlag
				if(model && model.id != ""){
					this.getTempalteName(model,model.id)  //获取模板名称
				}
			},
			//获取模板名称数据
			getTempalteName:function(model,id){
				Bus.$emit("FtoSon",model)
			},
			close:function(){
				this.$emit("close")
			},
			//递归函数：设置递归组件的选中状态
			setSelectedFlag:function(arr){
				for(var i=0;i<arr.length;i++){
					arr[i].selectFlag = false
					if(arr[i].childList && arr[i].childList.length){
						this.setSelectedFlag(arr[i].childList)
					}
				}
			},
		},
		directives: {
			focus: {
				inserted(el){
					el.focus()
				}
			}
		},
		beforeDestory(){
			Bus.$off()
		},
	}
</script>

<style scoped>
.tree li {
	cursor: pointer;
}
.titleName {
	height: 34px;
	width: 100%;
	padding-left: 2%;
	box-sizing: border-box;
	display: flex;
	align-items: center;
	position: relative;
}
.titleName:before {
	content: "";
	display: block;
	position: absolute;
	bottom: 0px;
	width: 96%;
	font-size: 0;
	line-height: 0;
}
.titleName .fa {
	width: 12px;
	height: 12px;
	display: inline-block;
	margin-left: 8px;
	margin-right: 4px;
}
.titleName span {
	width: 170px;
	white-space: nowrap;
	display:inline-block;
	vertical-align: text-bottom;
	overflow: hidden;
	text-overflow: ellipsis;
}
.titleName .fa-folder-j {
	background: url("../img/add.png");
	background-size: contain;
}
.titleName .fa-folder-open-j {
	background: url("../img/less.png");
	background-size: contain;
}
li {
	list-style: none;
}
ul {
	margin:0;
	padding:0;
	flex: 1;
}
.tag {
	height: 34px;
	border: 2px solid #09f;
	position: relative;
	background-color: #ebf7fa;
	color: #3d99b0;
	box-sizing: border-box;
}
input {
	width: 100%;
	height: 100%;
	border: none;modelItem
	outline: none;
}
img {
	width: 15px;
	height: 15px;
	margin: 0 5px;
	cursor: pointer;
}
.icon_operation {
	position: absolute;
	right: 5px;
}
</style>

<style>
</style>
