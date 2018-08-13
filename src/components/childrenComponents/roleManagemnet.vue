<template>
	<div class="roleManagement">
		角色管理界面
		<gz-popup v-model="isShow" w="800" h="200">
		 <button type="button" name="button" @click="openMaskLayer">取消</button>
		</gz-popup>
	</div>
</template>

<script>
	export default {	
		data(){
			return {
				isShow: true
			}
		},
		methods:{
			openMaskLayer:function(){
				this.isShow = !this.isShow
			}
		}
	}
</script>

<style scoped>
</style>

<style>
</style>
