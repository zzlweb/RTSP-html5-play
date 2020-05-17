<template>
    <video class="demo-video" ref="player" muted autoplay controls></video>
</template>
<script>
import flvjs from "flv.js";
export default {
    data () {
        return {
        id: "1",
        rtsp: "rtsp://wowzaec2demo.streamlock.net/vod/mp4:BigBuckBunny_115k.mov",
        player: null
        }
    },
    mounted () {
        if (flvjs.isSupported()) {
            let video = this.$refs.player;
            if (video) {
                this.player = flvjs.createPlayer({
                    // 类型
                    type: "flv",
                    // 指示数据源是否为活流
                    isLive: true,
                    url: `ws://localhost:8888/rtsp/${this.id}/?url=${this.rtsp}`,
                    // 指示流是否有音频跟踪
                    hasAudio:'true'
                });
                this.player.attachMediaElement(video);
                try {
                    this.player.load();
                    this.player.play();
                } catch (error) {
                    console.log(error);
                };
            }
        }
    },
    beforeDestroy () {
        this.player.destory();
    }
}
</script>
<style>
    .demo-video {
        width: 600px;
        height: 400px;
    }
</style>