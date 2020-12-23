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
			content: ''
		}
	},
	onUnload() {
		if(this.content) {
			let noteList = uni.getStorageSync('note_list'),
				newNote = {
					id: -1,
					content: this.content,
					updateTime: this.getDateText(new Date)
				}
			
			if(!noteList||noteList.length == 0) {
				newNote.id = 0;
				noteList = [newNote]
			}
			else {
				let lastNote = noteList[noteList.length - 1];
				newNote.id = lastNote.id + 1;
				noteList.push(newNote);
			}
			
			uni.setStorageSync('note_list', noteList);
		}
	},
	methods: {
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
