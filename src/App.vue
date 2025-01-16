<template>
  <div class="container">
    <AudioController /> 
    <AddSongForm />
    <MyPlaylist />
  </div>
</template>

<script>
import AudioController from './components/AudioController.vue';
import AddSongForm from './components/AddSongForm.vue';
import MyPlaylist from './components/MyPlaylist.vue';

export default {
  components: {
    AudioController,
    AddSongForm,
    MyPlaylist,
  },
};
</script>

<style>
/* Estilos generales */
.container {
  max-width: 800px;
  margin: auto;
  padding: 20px;
  background-color: #006d23;
  border-radius: 8px;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.3);
}

.title {
  text-align: center;
  color: #1db954;
}

/* Estilos para AudioController */
.audio-player {
  margin: 20px 0;
  text-align: center;
}

.audio-title {
  font-size: 1.2em;
  margin-bottom: 10px;
  color: #ffffff;
}

.controls {
  display: flex;
  align-items: center;
  justify-content: center;
}  

.play-pause {
  margin-right: 10px;
  padding: 10px 20px;
  background-color: #1db954;
  color: white;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  transition: background-color 0.3s;
}

.play-pause:hover {
  background-color: #1aa34a;
}

.play-pause:active {
  background-color: #148c3c;
}

.seek-bar {
  margin: 0 10px;
  width: 50%;
}

.time {
  font-size: 0.9em;
  color: #ffffff;
}

/* Estilos para AddSongForm */
.upload-method {
  margin: 20px 0;
}

.file-input, .upload-select {
  width: 100%;
  padding: 10px;
  margin: 10px 0;
  border: 1px solid #1db954;
  border-radius: 5px;
  background-color: #282828;
  color: #ffffff;
}

.url-input {
  width: 97.5%;
  padding: 10px;
  margin: 10px 0;
  border: 1px solid #1db954;
  border-radius: 5px;
  background-color: #282828;
  color: #ffffff;
}

.upload-button {
  padding: 10px 20px;
  background-color: #1db954;
  color: white;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  transition: background-color 0.3s;
}

.upload-button:hover {
  background-color: #1aa34a;
}

.upload-button:active {
  background-color: #148c3c;
}

/* Estilos para MyPlaylist */
.playlist-title {
  margin-top: 30px;
  text-align: center;
  color: #ffffff;
}

.playlist {
  list-style-type: none;
  padding: 0;
}

.playlist li {
  display: flex;
  justify-content: space-between;
  padding: 10px;
  border: 1px solid #1db954;
  border-radius: 5px;
  margin: 5px 0;
  background-color: #282828;
  color: #ffffff;
}

.playlist li.active {
  background-color: #108a3b;
}

.playlist li.broken {
  text-decoration: line-through;
  color: red;
}

.play-button, .remove-button {
  padding: 5px 10px;
  margin-left: 10px;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}

.play-button {
  background-color: #1db954;
  color: white;
}

.remove-button {
  background-color: #dc3545;
  color: white;
}

/* Botones de modo de repetición */
.repeat-mode {
  margin: 20px 0;
  text-align: center;
}

.repeat-mode button {
  padding: 10px 15px;
  background-color: #1db954;
  color: white;
  border: 2px solid black;
  border-radius: 20px;
  cursor: pointer;
  margin: 0 5px;
  transition: background-color 0.3s;
}

.repeat-mode button:hover {
  background-color: #1aa34a;
}

.repeat-mode button:active {
  background-color: #148c3c;
}

.repeat-mode button.activeMode {
  background-color: #006400;
}
</style>
