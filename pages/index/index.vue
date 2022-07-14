<template>
	<view class="content">
		<luch-audio :src="srcGet.src" @onError="onAudioError" :initAudio="initCb" autoplay @onPlay="audioPlay" :poster="srcGet.poster" :name="srcGet.name" :author="srcGet.author" :play.sync="audioToPlay"></luch-audio>
		<view style="height: 50px;"></view>
		<button @click="lastAudio">上一首</button>
		<button @click="audioHandle">{{audioToPlay ? '暂停' : '播放'}}</button>
		<button @click="nextAudio">下一首</button>
		<view>当前播放的歌曲：{{ srcGet.name }}</view>
		<view>
			播放列表：
			<view>
				<view v-for="item in srcCache" :key="item.src">{{ item.name }}</view>
			</view>
		</view>
	</view>
</template>

<script>
export default {
	data() {
		return {
			// 下方音乐文件，本人无版权，仅做本示例用，请勿他用。
			srcCache: [
				{ src: 'https://bjetxgzv.cdn.bspapp.com/VKCEYUGU-hello-uniapp/2cc220e0-c27a-11ea-9dfb-6da8e309e0d8.mp3', name: '歌曲名称1', author: '作者1' },
				{ src: 'https://downsc.chinaz.net/Files/DownLoad/sound1/202207/y984.mp3', name: '歌曲名称2', author: '作者2', poster: require('@/static/logo.png') },
				{ src: 'https://downsc.chinaz.net/Files/DownLoad/sound1/202207/y977.mp3', name: '歌曲名称3', author: '作者3' }
			],
			srcIndex: 0,
			audioToPlay: false
		};
	},
	computed: {
		srcGet() {
			return this.srcCache[this.srcIndex];
		}
	},
	methods: {
		onAudioError(e) {
			console.log('播放出错');
			console.log(e);
		},
		lastAudio() {
			if (this.srcIndex === 0) {
				this.srcIndex = this.srcCache.length - 1;
			} else {
				this.srcIndex--;
			}
		},
		// 下一首
		nextAudio() {
			if (this.srcIndex === this.srcCache.length - 1) {
				this.srcIndex = 0;
			} else {
				this.srcIndex++;
			}
		},
		initCb(context, data) {
			// 设置属性
			// context.loop = true
			console.log('initCb data', data);
			context.onCanplay(() => {
				console.log('context duration', context.duration);
			});
			context.onEnded(() => {
				this.nextAudio()
			})
		},
		audioPlay () {
			console.log('视频开始播放了');
		},
		audioHandle () {
			this.audioToPlay = !this.audioToPlay
		}
	}
};
</script>

<style></style>
