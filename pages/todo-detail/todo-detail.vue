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
			id: -1,
			content: ''
		}
	},
	onLoad(event) {
		this.id = event.id;
		this.initContent();
	},
	onUnload() {
		let todoList = uni.getStorageSync('todo_list');
		let todo = todoList.find(item => {
			return item.id == this.id
		})
		todo.content = this.content;
		
		uni.setStorageSync('todo_list', todoList);
	},
	onNavigationBarButtonTap(event) {
		if(event.index == 0) {
			uni.showModal({
				content: "确认删除待办？",
				success: (event) => {
					if(event.confirm) {
						let todoList = uni.getStorageSync('todo_list');
						let index = todoList.findIndex(item => {
							return item.id == this.id
						})
						
						todoList.splice(index, 1);
						uni.setStorageSync('todo_list', todoList);
						uni.navigateBack();
					}
				}
			})
		}
	},
	methods: {
		initContent() {
			let todoList = uni.getStorageSync('todo_list');
			let todo = todoList.find(item => {
				return item.id == this.id
			})
			
			this.content = todo.content;
		},
		contentInput(event) {
			this.content = event.detail.value;
		}
	}
}
</script>
