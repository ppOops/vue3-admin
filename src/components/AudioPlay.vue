<template>
   
  <div>
       
    <div class="play-icon" @click="changeAudioState">
      {{ play ? "暂停" : "播放" }}
      <!--       <el-icon size="20px"><CaretRight /></el-icon>     -->
    </div>

       
    <audio
      ref="audioRef"
      :src="src"
      controls="controls"
      class="audio"
    ></audio>
       
    <!-- <audio
          ref="audio"
          @pause="onPause"
          @play="onPlay"
          @timeupdate="onTimeupdate"
          @loadedmetadata="onLoadedmetadata"
          :src="src"
          controls="controls"
        ></audio> -->
     
  </div>
</template>

<script>
import { reactive, ref, toRefs, watch } from "vue";
// import { CaretRight } from "@element-plus/icons-vue";

export default {
  props: {
    src: {
      type: String,
    },
    isPlay: {
      type: Boolean,
      default: false,
    },
    id: {
      // 标识
      type: Number,
      default: 1

    },
  },
  components: {
    // CaretRight,
  },
  setup(props, { emit }) {
    const audioRef = ref(null);
    const state = reactive({
      play: false,
    });

    const playAudio = () => {
        console.log('play')
        state.play = true;
        emit("changeStatus", props.id, true);
    };

    const pauseAudio = () => {
        console.log('pause')
        state.play = false;
        emit("changeStatus", props.id, false);
    };

    const changeAudioState = () => {
        console.log(state.play)
        state.play ? pauseAudio() : playAudio()
    }

    const onPause = () => {
        pauseAudio()
        // console.log('pause')
        // state.play = false;
        // emit("changeStatus", props.id, false);
    }; 

    const onPlay = () => {
        playAudio();
        // console.log('play')
        // state.play = true;
        // emit("changeStatus", props.id, true);
    };

    watch(
      () => props.isPlay,
      (newV) => {
        state.play = newV;
        if (state.play) {
          audioRef.value.play();
        } else {
          try {
            audioRef.value.pause();
          } catch (error) {
            console.log(error)
          }
        }
      },
      { immediate: true }
    );

    return {
      ...toRefs(state),
      audioRef,
      changeAudioState,
      playAudio,
      pauseAudio,
      onPause,
      onPlay,
    };
  },
};
</script>

<style scoped>
.play-icon {
  cursor: pointer;
}

.audio {
  width: 260px;
  height: 34px;
}
</style>
