<style>
.page {
	width: 100%;
	height: 100%;
	background-color: #F9F9F9;
}

.todo-list {
	width: 100%;
	padding: 32rpx;
}

.todo {
	width: 100%;
	background-color: white;
	padding: 40rpx;
	transition: all 0.1s;
	display: flex;
	align-items: center;
}

.todo:active {
	background-color: #eee;
}

.todo + .todo {
	margin-top: 24rpx;
}

.todo-completed {
	opacity: 0.5;
}

.todo-completed .todo-title {
	text-decoration: line-through;
}

.todo-status {
	width: 15%;
}

.todo-title {
	width: 85%;
	font-size: 30rpx;
	white-space: nowrap;
	overflow: hidden;
	text-overflow: ellipsis;
}

.add-todo-title {
	color: #FFA500;
}

.todo-status checkbox {
	transform: scale(0.6, 0.6);
}
</style>

<template>
	<scroll-view class="page" scroll-y>
		<view class="todo-list">
			<view class="todo" @click="addTodo">
				<view class="todo-title add-todo-title">＋ 点击新增</view>
			</view>
			<view
				class="todo"
				v-for="item in todoList"
				:key="item.id"
				@click="toTodoDetail(item.id)"
				:class="{ 'todo-completed': item.completed }"
			>
				<view class="todo-status" @click.stop>
					<checkbox
						value="completed"
						:checked="item.completed"
						color="#FFA500"
						@click="changeTodoStatus(item.id)"
					/>
				</view>
				<view class="todo-title">{{ item.content }}</view>
			</view>
		</view>
	</scroll-view>
</template>

<script>
export default {
	data() {
		return {
			todoList: []
		}
	},
	onShow() {
		let todoList = uni.getStorageSync('todo_list');
		if(todoList) {
			this.todoList = todoList;
		}
	},
	methods: {
		changeTodoStatus(id) {
			let todo = this.todoList.find(item => {
				return item.id == id
			})
			
			todo.completed = !todo.completed;
			
			uni.setStorageSync('todo_list', this.todoList);
		},
		toTodoDetail(id) {
			uni.navigateTo({
				url: '/pages/todo-detail/todo-detail?id=' + id,
				animationType: 'slide-in-right',
				animationDuration: 200
			})
		},
		addTodo() {
			uni.navigateTo({
				url: '/pages/add-todo/add-todo',
				animationType: 'slide-in-bottom',
				animationDuration: 200
			})
		}
	}
}
</script>
