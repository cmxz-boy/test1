<template>
  <div class="hello">
    <el-tabs v-model="activeName2" :tab-position="tabPosition" @tab-click="handleClick">
      <el-tab-pane label="用户管理" name="first"><user-management ref="userManagement"></user-management></el-tab-pane>
      <el-tab-pane label="配置管理" name="second"><configuration-management></configuration-management></el-tab-pane>
      <el-tab-pane label="角色管理" name="third"><role-management></role-management></el-tab-pane>
      <el-tab-pane label="权限管理" name="fourth"><authority-management></authority-management></el-tab-pane>
      <el-tab-pane label="个人中心" name="fiveth"><person-management></person-management></el-tab-pane>
      <el-tab-pane label="历史记录" name="sixth"><history-memory></history-memory></el-tab-pane>
      <el-tab-pane label="日日日历" name="seventth"><calendar-management></calendar-management></el-tab-pane>
      <el-tab-pane label="任务列表" name="eighth"><task-list></task-list></el-tab-pane>
      <el-tab-pane label="活动中心" name="naneth"><active-center></active-center></el-tab-pane>
      <el-tab-pane label="登录系统" name="tenth"><login-system></login-system></el-tab-pane>
      <el-tab-pane label="安全维护" name="eleven">安全维护</el-tab-pane>
      <el-tab-pane label="主题中心" name="twelve">主题中心</el-tab-pane>
      <el-tab-pane label="任务补偿" name="thirteen">任务补偿</el-tab-pane>
      <el-tab-pane label="密码修改" name="fourteen">密码修改</el-tab-pane>
      <el-tab-pane label="安全认证" name="fifteen">安全认证</el-tab-pane>
    </el-tabs>
    <textarea v-moel="textbox" v-if="textShow" @keydown.enter="handleChange" id="textbox" cols="30" rows="1"></textarea>
  </div>
</template>

<script>
import userManagement from './childrenComponents/userManagement'   //用户管理
import configurationManagement from './childrenComponents/configurationManagement'  //配置管理
import roleManagement from './childrenComponents/roleManagemnet'  //角色管理
import authorityManagement from './childrenComponents/authorityManagement'  //权限管理
import personManagement from './childrenComponents/personManagement'  //个人中心
import historyMemory from './childrenComponents/historyMemory'  //历史记录
import calendarManagement from './childrenComponents/calendarManagement'  //日历管理
import taskList from './childrenComponents/taskList'   //任务列表
import activeCenter from './childrenComponents/activeCenter'  //活动中心
import loginSystem from './childrenComponents/loginSystem'   //登录系统

export default {
  name: 'HelloWorld',
  data () {
    return {
      textbox: '',
      textShow: false,
      activeName2:'',
      tabPosition: 'left'
    }
  },
  components: {
    userManagement,  //用户管理
    configurationManagement,  //配置管理
    roleManagement, //角色管理
    authorityManagement,  // 权限管理
    personManagement,   //个人中心,
    historyMemory,   //历史记录
    calendarManagement,  //日历管理
    taskList,   // 任务列表
    activeCenter,  //活动中心
    loginSystem,  // 登录系统
  },
  methods: {
   handleChange:function(e){
    var _e = e || window.event
    if(_e.keyCode == 13){
      _e.returnValue = false
      _e.preventDefault()
      this.textbox = ''
      // $(this).on('blur')
    }
   },
   handleClick(tab, event) {
      // console.log(tab, event);
   }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
</style>

<style>
 
</style>
