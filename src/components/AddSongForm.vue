<template>
    <div class="container">
      <h1 class="title">Spotify</h1>
  
      <!-- Muestra el audio subido -->
      <figure v-if="audioSrc" class="audio-player">
        <figcaption class="audio-title">Sonando: {{ audioFileName }}</figcaption>
        <audio
          ref="audioElement"
          :src="audioSrc"
          @timeupdate="updateTime"
          @ended="handleAudioEnd"
          @loadedmetadata="updateDuration"
        ></audio>
        <div class="controls">
          <button class="play-pause" @click="isPlaying ? pauseAudio() : playAudio()">
            {{ isPlaying ? 'Pausar' : 'Reanudar' }}
          </button>
          <input type="range" v-model="currentTime" :max="duration" @input="seekAudio" class="seek-bar" />
          <span class="time">{{ formatTime(currentTime) }} / {{ formatTime(duration) }}</span>
        </div>
      </figure>
  
      <!-- Botones de modo de repetición -->
      <div class="repeat-mode">
        <h3>Modo de Repetición</h3>
        <button @click="changeRepeatMode('none')" :class="{ activeMode: repeatMode === 'none' }">No Repetir</button>
        <button @click="changeRepeatMode('song')" :class="{ activeMode: repeatMode === 'song' }">Repetir Canción</button>
        <button @click="changeRepeatMode('list')" :class="{ activeMode: repeatMode === 'list' }">Repetir Lista</button>
      </div>
  
      <div class="upload-method">
        <select v-model="metodoSubida" @change="resetInputs" class="upload-select">
          <option value="url">Subir por URL</option>
          <option value="file">Subir desde el ordenador</option>
        </select>
  
        <div v-if="metodoSubida === 'url'" class="url-upload">
          <input type="text" v-model="audioUrl" placeholder="Introduce la URL del audio" class="url-input" />
        </div>
  
        <div v-if="metodoSubida === 'file'" class="file-upload">
          <input type="file" @change="handleFileUpload" class="file-input" />
        </div>
      </div>
  
      <h2 class="playlist-title">Lista de Reproducción</h2>
      <ul class="playlist">
        <li
          v-for="(item, index) in playlist"
          :key="index"
          :class="{ active: index === currentIndex, broken: item.broken }"
        >
          {{ item.title }}
          <button @click="playItem(index)" :disabled="item.broken" class="play-button">Reproducir</button>
          <button @click="removeItem(index)" class="remove-button">Eliminar</button>
        </li>
      </ul>
    </div>
  </template>
  
  <script>
  import { ref, reactive, nextTick } from "vue";
  
  export default {
    setup() {
      const audioSrc = ref(null);
      const audioFileName = ref("");
      const isPlaying = ref(false);
      const currentTime = ref(0);
      const duration = ref(0);
      const metodoSubida = ref("url");
      const audioUrl = ref("");
      const audioTitle = ref("");
      const playlist = reactive([]);
      const currentIndex = ref(-1);
      const repeatMode = ref("none");
      const audioElement = ref(null);
  
      const playAudio = () => {
        if (audioElement.value) {
          audioElement.value.play();
          isPlaying.value = true;
        }
      };
  
      const pauseAudio = () => {
        if (audioElement.value) {
          audioElement.value.pause();
          isPlaying.value = false;
        }
      };
  
      const updateTime = () => {
        if (audioElement.value) {
          currentTime.value = audioElement.value.currentTime;
          duration.value = audioElement.value.duration;
        }
      };
  
      const seekAudio = () => {
        if (audioElement.value) {
          audioElement.value.currentTime = currentTime.value;
        }
      };
  
      const handleAudioEnd = () => {
        if (repeatMode.value === "song") {
          playAudio();
        } else if (repeatMode.value === "list") {
          if (currentIndex.value + 1 < playlist.length) {
            playItem(currentIndex.value + 1);
          } else {
            playItem(0);
          }
        } else if (repeatMode.value === "none") {
          if (currentIndex.value + 1 < playlist.length) {
            playItem(currentIndex.value + 1);
          } else {
            isPlaying.value = false;
          }
        }
      };
  
      const changeRepeatMode = (mode) => {
        repeatMode.value = mode;
      };
  
      const subirPorUrl = () => {
        if (audioUrl.value.trim() !== "") {
          const title = audioTitle.value.trim() || audioUrl.value.split("/").pop();
          playlist.push({
            title,
            src: audioUrl.value,
            broken: false,
          });
          audioUrl.value = "";
          audioTitle.value = "";
        } else {
          alert("Por favor, introduce una URL válida.");
        }
      };
  
      const subirPorArchivo = (event) => {
        const file = event.target.files[0];
        if (file) {
          const src = URL.createObjectURL(file);
          const title = audioTitle.value.trim() || file.name;
          playlist.push({
            title,
            src,
            broken: false,
          });
          audioTitle.value = "";
          event.target.value = null;
        } else {
          alert("Por favor, selecciona un archivo válido.");
        }
      };
  
      const handleFileUpload = (event) => {
        subirPorArchivo(event);
      };
  
      const resetInputs = () => {
        audioUrl.value = "";
        audioTitle.value = "";
      };
  
      const playItem = (index) => {
        const item = playlist[index];
        if (!item.broken) {
          audioSrc.value = item.src;
          audioFileName.value = item.title;
          currentIndex.value = index;
          nextTick(() => {
            if (audioElement.value) {
              playAudio();
            }
          });
        }
      };
  
      const removeItem = (index) => {
        if (index === currentIndex.value) {
          audioSrc.value = null;
          audioFileName.value = "";
          isPlaying.value = false;
          currentIndex.value = -1;
        }
        playlist.splice(index, 1);
      };
  
      const formatTime = (seconds) => {
        if (isNaN(seconds)) {
          return "00:00";
        }
        const minutes = Math.floor(seconds / 60);
        const secs = Math.floor(seconds % 60);
        return `${String(minutes).padStart(2, "0")}:${String(secs).padStart(2, "0")}`;
      };
  
      return {
        audioSrc,
        audioFileName,
        isPlaying,
        currentTime,
        duration,
        metodoSubida,
        audioUrl,
        audioTitle,
        playlist,
        currentIndex,
        repeatMode,
        audioElement,
        playAudio,
        pauseAudio,
        updateTime,
        seekAudio,
        handleAudioEnd,
        changeRepeatMode,
        subirPorUrl,
        handleFileUpload,
        resetInputs,
        playItem,
        removeItem,
        formatTime,
      };
    },
  };
  </script>
  