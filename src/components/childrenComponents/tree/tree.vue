<template>
	<div class="tree" v-if="modelItem.deletedFlag">
		<li>
			<div class="titleName" :class="modelItem.selectFlag ? 'tag' : ''">
				<div class="space" :style="{width:modelItem.label*10+ 'px'}" v-if="modelItem.label"></div>
				<i class="fa" v-if="isFolder" :class="modelItem.OpenFlag? 'fa-folder-open-j' : 'fa-folder-j'"></i>
				<i class="fa" v-if="!isFolder"></i>	
				<input ref="input" v-if="modelItem.EditAble" type="text" v-model="modelItem.name" @click.stop="stopBubbing" @blur="blurEvent(modelItem)" v-focus>
				<span v-else :title="model.name">{{model.name}}</span>
				<div class="icon_operation">
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
		props:["model","treeModel","modelTree","items","getTemplateLists","taddTemplateListFlag"],
		data(){
			return {
				mymodelTree: this.modelTree, //tree全部数据
				clicModelList: {}, //暂存当前点击的tree列表
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
			isFolder:function(){
				return this.modelItem.childList && this.modelItem.childList.length
			},
			modelItem:function(){
				return this.model
			}
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
			stopBubbing:function(){},  //点击模板名称input阻止冒泡
			blurEvent:function(modelItem){
				model.EditAble = false
				if(modelItem.id && modelItem.id != ""){
					//修改name离焦保存
					if(modelItem.name && modelItem.name != ""){
						//发请求进行修改
						console.log(this,'this')
						alert('修改成功')
					}
				}
			},
			//添加
			addTemplateChilkldList:function(){},
			//编辑
			editTemplateList:function(){},
			//删除
			deletModelList:function(){},
		}
	}
</script>

<style scoped>
.tag {
	height: 34px;
	border: 2px solid #09f;
	position: relative;
	background-color: #ebf7fa;
	color: #3d99b0;
	box-sizing: border-box;
}
</style>

<style>
</style>
