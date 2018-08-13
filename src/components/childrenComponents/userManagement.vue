<template>
	<div class="userManagement">
		用户管理
		<h3>添加删除列表数据</h3>
		<input v-model="lis" type="text" placeholder="请输入内容" @keydown.enter="handlePush">
		<ul class="listTree">
			<li v-for="(item,index) in taskList">{{item.label}} <span @click="handleDelete(index)">x</span></li>
		</ul>
		<br>
		<!-- table表格 -->
		<el-table :data="tableData" border>
			<el-table-column
			 v-for="titem in tableList"
			 :key="titem.code"
			 :prop="titem.code"
			 :label="titem.label" 
			 resizable>
			</el-table-column>
		</el-table>
		<br><br>
		<el-table :data="testData" border>
      <el-table-column prop="num" label="序号"></el-table-column>
      <el-table-column prop="name" label="姓名"></el-table-column>
    </el-table>

   <br><br>
   <!-- 过渡 -->
   <h3>过渡</h3>
   <transition name="fade" mode="out-in">
		  <button v-if="on" key="on" @click="on = false">on</button>
		  <button else key="off" @click="on = true">off</button>
	 </transition>
	 <!-- 多个组件的过渡 -->

	 <!-- 不带表头的table -->
	 <!-- :show-header='false'  默认true -->
	 <h3>不带表头的table</h3>
	 <el-table :data="testData" :show-header='false' border>
     <el-table-column prop="num" label=""></el-table-column>
     <el-table-column prop="name" label=""></el-table-column>
   </el-table>
	</div>
</template>

<script>
	export default {
		data(){
			return {
				taskList: [{label:'哈哈哈'}],  //li列表
				lis: '',  //input
			
				// table数据				
				tableData:[
					{
						date: '2016-08-09',
						num: 20,
						name: '李四',
						age: 20
					},
					{
						date: '2016-05-19',
						num: 23,
						name: '张三',
						age: 20
					},
				],
				tableList: [
					{
						code: 'date',
						label: '日期'
					},
					{
						code: 'num',
						label: '数量'
					},
					{
						code: 'name',
						label: '名称'
					},
					{
						code: 'age',
						label: '年龄'
					}
				],

				testData: [
					{
						num: 12344566,
						name: 'authorityManagement'
					},
					{
						num: 21424568,
						name: 'authorityManagement'
					}
				],
				// 过渡
				on: false,
			}
		},
		methods:{
			handlePush:function(){
				let label = this.lis.trim()
				if(label){
					this.taskList.push({label:label})
					this.lis = ''
				}
			},
			handleDelete:function(index){
				console.log(index)
				this.taskList.splice(index,1)
			},
			hanldeInput:function(val){
				console.log(val,this.u)
			}
		}
	}
</script>

<style scoped>
.listTree {
	width: 184px;
}
.listTree li {
	heiht: 25px;
	line-height: 25px;
}
.listTree span {
	padding: 0px 2px;
	border: 1px solid #000;
}
</style>

<style>
.fade-enter-active, .fade-leave-active {
  transition: opacity .5s;
}
.fade-enter, .fade-leave-to /* .fade-leave-active below version 2.1.8 */ {
  opacity: 0;
}
</style>
