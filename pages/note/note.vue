<style>
.page {
	width: 100%;
	height: 100%;
	background-color: #F9F9F9;
}

.note-list {
	width: 100%;
	padding: 32rpx;
}

.note {
	width: 100%;
	background-color: white;
	padding: 40rpx;
	transition: all 0.1s;
}

.note:active {
	background-color: #eee;
}

.note + .note {
	margin-top: 24rpx;
}

.note-title {
	width: 100%;
	font-size: 30rpx;
	white-space: nowrap;
	overflow: hidden;
	text-overflow: ellipsis;
}

.note-update-time {
	color: #999;
	font-size: 24rpx;
	margin-top: 16rpx;
}

.add-note-title {
	color: #FFA500;
}
</style>

<template>
	<scroll-view class="page" scroll-y>
		<view class="note-list">
			<view
				class="note"
				v-for="item in noteList"
				:key="item.id"
				@click="toNoteDetail(item.id)"
			>
				<view class="note-title">{{ item.content }}</view>
				<view class="note-update-time">{{ item.updateTime }}</view>
			</view>
			<view class="note" @click="addNote">
				<view class="note-title add-note-title">＋ 点击新增</view>
			</view>
		</view>
	</scroll-view>
</template>

<script>
export default {
	data() {
		return {
			noteList: []
		}
	},
	onShow() {
		let noteList = uni.getStorageSync('note_list');
		if(noteList) {
			this.noteList = noteList;
		}
	},
	methods: {
		toNoteDetail(id) {
			uni.navigateTo({
				url: '/pages/note-detail/note-detail?id=' + id,
				animationType: 'slide-in-right',
				animationDuration: 200
			})
		},
		addNote() {
			uni.navigateTo({
				url: '/pages/add-note/add-note',
				animationType: 'slide-in-bottom',
				animationDuration: 200
			})
		}
	}
}
</script>
