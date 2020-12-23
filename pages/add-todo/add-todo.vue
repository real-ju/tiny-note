<style>
.page {
	width: 100%;
	height: 100%;
	padding: 32rpx;
}

textarea {
	min-height: 200rpx;
	font-size: 32rpx;
}
</style>

<template>
	<scroll-view class="page" scroll-y>
		<textarea
			class="textarea"
			:value="content"
			placeholder="记待办"
			:maxlength="1000"
			:focus="true"
			:auto-height="true"
			@input="contentInput"
		/>
	</scroll-view>
</template>

<script>
export default {
	data() {
		return {
			content: ''
		}
	},
	onUnload() {
		if(this.content) {
			let todoList = uni.getStorageSync('todo_list'),
				newTodo = {
					id: -1,
					content: this.content,
					completed: false
				}
			
			if(!todoList||todoList.length == 0) {
				newTodo.id = 0;
				todoList = [newTodo]
			}
			else {
				let lastTodo = todoList[0];
				newTodo.id = lastTodo.id + 1;
				todoList.splice(0, 0, newTodo);
			}
			
			uni.setStorageSync('todo_list', todoList);
		}
	},
	methods: {
		contentInput(event) {
			this.content = event.detail.value;
		}
	}
}
</script>
