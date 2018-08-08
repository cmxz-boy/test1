<template>
	<div class="configurationManagement">
		配置管理界面
		<el-button type="text" @click="dialogVisible = true">点击打开 Dialog</el-button>

		<el-dialog
		  title="新建入库单"
		  :visible.sync="dialogVisible"
		  width="90%"
		  :fullscreen="true"
		  :hide-on-click="false"
		  :before-close="handleClose">
			<div class="clearfix">
				<div class="left clearfix">
					<div class="goodsWatch clearfix">
				 		<el-button class="btn" type="primary" @click="handleWatchShow">物品一览</el-button>
				 	</div>
					<div class="table1">
						<el-table
					    :data="tableData1"
					    height="250"
					    style="width: 100%">
					    <el-table-column
					      prop="name"
					      label="名称">
					    </el-table-column>
					    <el-table-column
					      prop="brand"
					      label="品牌">
					    </el-table-column>
					    <el-table-column
					      prop="specification"
					      label="规格">
					    </el-table-column>
					    <el-table-column
				        prop="unitName"
				        label="单位">
				      </el-table-column>
				      <el-table-column
				        prop="num"
				        label="数量">
				      </el-table-column>
				      <el-table-column
				        prop="operation"
				        label="操作">
				        <template slot-scope="scope">
					       <div class="operationL">
					       	 <el-button
					         mini
					         type="primary"
					         icon="el-icon-edit"
					         @click="handleMemo(scope.$index, scope.row,tableData1,tableData)"
					         circle></el-button>
					        <el-button
					         mini
					         type="danger"
					         icon="el-icon-delete"
					         @click="handleDelete(scope.$index, scope.row,tableData1,tableData)"
					         circle></el-button>
					         <el-input autofocus v-if="isMemoShow === scope.$index" v-show="memoShow"  type="text" placeholder="请添加备注" v-model="scope.row.memo"></el-input>
					       </div>
					      </template>
				      </el-table-column>
					  </el-table>
					</div>
					 <span slot="footer" class="dialog-footer">
				    <el-button @click="dialogVisible = false">取 消</el-button>
				    <el-button type="primary" @click="dialogVisible = false">确 定</el-button>
		  		 </span>
				</div>
				<div class="right" v-show="rightShow">
					<input class="right_search" type="text" placeholder="请输入搜索名称">
					<div class="table2">
						 <el-table
				      :data="tableData"
				      :span-method="arraySpanMethod"
				      style="width: 100%">
				      <el-table-column
				        label="规格"
				        width="120">
				        <template slot-scope="scope">
				        	<div class="imgTop clearfix">
					        	<div class="imgTop_left"><img src="" alt=""></div>
					        	<div class="imgTop_right">
					        		<p>{{scope.row.name}}</p>
					        		<p>品牌: <span>{{scope.row.brand}}</span></p>
					        		<P>所属类目：<span>{{scope.row.seriviceName}}</span></P>
					        	</div>
					        </div>
					        <el-table
					         :border="false"
					         :data="scope.row.tableData_inner"
					         :show-header='false'
					         style="width: 100%">
					         	 <el-table-column
							        prop="specification"
							        width="120">
							       </el-table-column>
							       <el-table-column
							        prop="unitName"
							        width="80">
							      </el-table-column>
							      <el-table-column
							        prop="num"
							        width="80">
							      </el-table-column>
							      <el-table-column
							        prop="operation">
							        <template slot-scope="scopeT">
								        <el-button
								          size="mini"
								          type="primary"
								          :disabled="!scopeT.row.imgFlag"
								          @click="handleEdit(scopeT.$index, scopeT.row,scope.row.tableData_inner,tableData1)">添加</el-button>
								      </template>
							      </el-table-column>
					        </el-table>
				        </template>
				      </el-table-column>
				      <el-table-column
				        prop="unitName"
				        label="单位"
				        width="80">
				      </el-table-column>
				      <el-table-column
				        prop="num"
				        label="数量"
				        width="80">
				      </el-table-column>
				      <el-table-column
				        prop="operation"
				        label="操作">
				      </el-table-column>
				    </el-table>
					</div>
				</div>
			</div>
		 <!--  <span slot="footer" class="dialog-footer">
		    <el-button @click="dialogVisible = false">取 消</el-button>
		    <el-button type="primary" @click="dialogVisible = false">确 定</el-button>
		  </span> -->
		</el-dialog>
		
	</div>
</template>

<script>
	export default {
		data(){
			return {
				dialogVisible: false,
				tableData1: [],
        rightShow: false,  //物品一览显示
        tableData3: [],
        tableData: [
        {
        	name: 'DMG AAAA',
        	brand: '33',
        	seriviceName: '护理品/冲牙器',
        	tableData_inner:[
	        	{
	        		name: 'DMG AAAA',
		        	brand: '33',
		        	seriviceName: '护理品/冲牙器',
	        		specification: '20*10',
		          unitName: '箱',
		          num: 100,
		          imgFlag: true,
		          memo:'',
		        },
		        {
		        	name: 'DMG AAAA',
		        	brand: '33',
		        	seriviceName: '护理品/冲牙器',
	        		specification: '20*10',
		          unitName: '个',
		          num: 600,
		          imgFlag: true,
		          memo:'',
		        }
        	]
        }, 
        {
        	name: 'BBBB',
        	brand: '66',
        	seriviceName: '护理品/yagao',
          tableData_inner:[
	        	{
	        		name: 'BBBB',
		        	brand: '66',
		        	seriviceName: '护理品/yagao',
	        		specification: '60*1',
		          unitName: '箱',
		          num: 20,
		          imgFlag: true,
		          memo:'',
		        }
        	]
        }],
        memoShow: false,  //备注显示或隐藏
        isMemoShow: '',
      }
		},
		methods:{
			//弹窗关闭
			handleClose(done) {
        this.$confirm('确认关闭？')
          .then(_ => {
            done();
          })
          .catch(_ => {});
      },
      //物品一览显示和隐藏
      handleWatchShow(){
      	this.rightShow = !this.rightShow
      	if(this.rightShow){
      		$('.left').css('width','60%')
      	}else{
      		$('.left').css('width','100%')
      	}
      },
      //合并单元格
      arraySpanMethod({ row, column, rowIndex, columnIndex }){
      	// console.log(row, column, rowIndex, columnIndex,11)
      	return [1,4]
      },
      //编辑按钮
      handleEdit(index, row,rightList,leftList) {
        // console.log(index, row,rightList,leftList);
        row.imgFlag = false
        leftList.push(row)
        // console.log('cur')
      },
      //添加备注*****
      handleMemo(index,row){
      	this.isMemoShow = index
      	this.memoShow = !this.memoShow
      },
      //删除添加的数据
      handleDelete(index,row,leftList,rightList){
      	console.log(index,row,leftList,rightList)
      	rightList.forEach((item)=>{
      		console.log(item,'item')
      		item.tableData_inner.forEach((ite)=>{
      			if(ite == row){
      				ite.imgFlag = true
      			}
      		})
      	})
      	leftList.splice(index,1)
      },
		}
	}
</script>

<style scoped>
.goodsWatch .btn {
	float: right;
	margin-bottom: 10px;
}
.left {
	float:left;
	width: 100%;
}
.right {
	float:right;
	width: 40%;
	height:500px;
	padding: 15px;
	box-sizing:border-box;
}
.table2 {
	border: 1px solid #ccc;	
}
.imgTop {
	width: 100%;
	height: 72px;
	padding: 8px;
	background-color: #ecf2ff;
} 
.imgTop_left {
	width:56px;
	height: 56px;
	background-color: #eee;
	float: left;
}
.imgTop_right {
	float:left;
	margin-left:15px;
	line-height: 18px;
}
.dialog-footer {
	margin-top: 25px;
	float:right;
}
.right_search {
	margin-bottom: 20px;
}
.table1 .el-button.is-circle {
	padding: 8px;
}
.operationL {
	margin-top: 8px;
	position: relative;
}
</style>

<style> 
.configurationManagement .el-dialog__body { 
	padding: 5px 20px;
}
.configurationManagement .table2 .cell {
	display:block;
	height: 100%;
}
.configurationManagement .table1 td:last-child .cell {
	// position: relative;
	overflow: visible;
}  
.configurationManagement .table1 .el-input {
	position: absolute;
	left: -195px;
	top:38px;
}
.configurationManagement .table1 .el-input__inner {
	height: 32px;
	line-height: 32px;
	width: 230px;
}
.configurationManagement .table1 .el-table__body td {
	height: 57px;
	padding: 0;
	box-sizing:border-box;
}
</style>
