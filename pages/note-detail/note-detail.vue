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
			placeholder="记事"
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
		let noteList = uni.getStorageSync('note_list');
		let note = noteList.find(item => {
			return item.id == this.id
		})
		note.content = this.content;
		note.updateTime = this.getDateText(new Date);
		
		uni.setStorageSync('note_list', noteList);
	},
	onNavigationBarButtonTap(event) {
		if(event.index == 0) {
			uni.showModal({
				content: "确认删除便签？",
				success: (event) => {
					if(event.confirm) {
						let noteList = uni.getStorageSync('note_list');
						let index = noteList.findIndex(item => {
							return item.id == this.id
						})
						
						noteList.splice(index, 1);
						uni.setStorageSync('note_list', noteList);
						uni.navigateBack();
					}
				}
			})
		}
	},
	methods: {
		initContent() {
			let noteList = uni.getStorageSync('note_list');
			let note = noteList.find(item => {
				return item.id == this.id
			})
			
			this.content = note.content;
		},
		getDateText(date) {
			let month = date.getMonth() + 1,
				day = date.getDate(),
				hour = date.getHours(),
				minute = date.getMinutes();
			
			return `${month}月${day}日 ${String(hour).padStart(2, '0')}:${String(minute).padStart(2, '0')}`
		},
		contentInput(event) {
			this.content = event.detail.value;
		}
	}
}
</script>
