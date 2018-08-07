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
				<div class="left">
					<div class="goodsWatch clearfix">
				 		<el-button class="btn" type="primary" @click="handleWatchShow">物品一览</el-button>
				 	</div>
					<div class="table1">
						<el-table
					    :data="tableData1"
					    height="250"
					    border
					    style="width: 100%">
					    <el-table-column
					      prop="date"
					      label="日期"
					      width="180">
					    </el-table-column>
					    <el-table-column
					      prop="name"
					      label="姓名"
					      width="180">
					    </el-table-column>
					    <el-table-column
					      prop="address"
					      label="地址">
					    </el-table-column>
					  </el-table>
					</div>
				</div>
				<div class="right" v-show="rightShow">
					<input type="text" placeholder="请输入搜索名称">
					<div class="table2">
						 <el-table
				      :data="tableData"
				      style="width: 100%">
				      <el-table-column
				        prop="date"
				        label="日期"
				        width="180">
				        <template slot-scope="scope">
				        	<div class="imgTop">
					        	<div><img src="" alt=""></div>
					        </div>
					        <el-table
					         :data="tableData3"
					         style="width: 100%">
					         	 <el-table-column
							        prop="date"
							        label="日期"
							        width="180">
							       </el-table-column>
					        </el-table>
				        </template>
				      </el-table-column>
				      <el-table-column
				        prop="name"
				        label="姓名"
				        width="80">
				      </el-table-column>
				      <el-table-column
				        prop="address"
				        label="地址">
				      </el-table-column>
				    </el-table>
					</div>
				</div>
			</div>
		  <span>这是一段信息</span>
		  <span slot="footer" class="dialog-footer">
		    <el-button @click="dialogVisible = false">取 消</el-button>
		    <el-button type="primary" @click="dialogVisible = false">确 定</el-button>
		  </span>
		</el-dialog>
	</div>
</template>

<script>
	export default {
		data(){
			return {
				dialogVisible: false,
				tableData1: [{
          date: '2016-05-03',
          name: '王小虎',
          address: '上海市普陀区金沙江路 1518 弄'
        }, {
          date: '2016-05-02',
          name: '王小虎',
          address: '上海市普陀区金沙江路 1518 弄'
        }, {
          date: '2016-05-04',
          name: '王小虎',
          address: '上海市普陀区金沙江路 1518 弄'
        }, {
          date: '2016-05-01',
          name: '王小虎',
          address: '上海市普陀区金沙江路 1518 弄'
        }, {
          date: '2016-05-08',
          name: '王小虎',
          address: '上海市普陀区金沙江路 1518 弄'
        }, {
          date: '2016-05-06',
          name: '王小虎',
          address: '上海市普陀区金沙江路 1518 弄'
        }, {
          date: '2016-05-07',
          name: '王小虎',
          address: '上海市普陀区金沙江路 1518 弄'
        }],
        rightShow: false,  //物品一览显示
        tableData3: [{
          date: '2016-05-03',
          name: '王小虎',
          address: '上海市普陀区金沙江路 1518 弄'
        }, {
          date: '2016-05-02',
          name: '王小虎',
          address: '上海市普陀区金沙江路 1518 弄'
        }, {
          date: '2016-05-04',
          name: '王小虎',
          address: '上海市普陀区金沙江路 1518 弄'
        }],
        tableData: []
      }
		},
		methods:{
			handleClose(done) {
        this.$confirm('确认关闭？')
          .then(_ => {
            done();
          })
          .catch(_ => {});
      },
      handleWatchShow(){
      	this.rightShow = !this.rightShow
      	if(this.rightShow){
      		$('.left').css('width','60%')
      	}else{
      		$('.left').css('width','100%')
      	}
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
</style>

<style> 
.el-dialog__body {
	padding: 5px 20px;
}
</style>
