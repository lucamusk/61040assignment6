<script setup lang="ts">
import { fetchy } from "@/utils/fetchy";
import { ref } from "vue";
const emit = defineEmits(["refreshPlaylists"]);

let dialogOpen = ref(false);
let name = ref("");

const setDialog = (state: boolean) => {
  dialogOpen.value = state;
};

const addPlaylist = async (playlistName: string) => {
  try {
    await fetchy("api/compilation/personal/", "POST", {
      body: { name: playlistName },
    });
  } catch (_) {
    alert("Playlist with that name already exists!");
    return;
  }
  emit("refreshPlaylists");
  name.value = "";
  setDialog(false);
};
</script>

<template>
  <div @click="setDialog(!dialogOpen)">
    <svg xmlns="http://www.w3.org/2000/svg" width="100" height="100" class="bi bi-plus" viewBox="0 0 16 16" fill="currentColor">
      <path d="M8 4a.5.5 0 0 1 .5.5v3h3a.5.5 0 0 1 0 1h-3v3a.5.5 0 0 1-1 0v-3h-3a.5.5 0 0 1 0-1h3v-3A.5.5 0 0 1 8 4z" />
    </svg>
  </div>
  <section :class="{ open: dialogOpen, closed: !dialogOpen }">
    <form @submit.prevent="addPlaylist(name)">
      <input id="name" v-model="name" placeholder="Write a name for the playlist!" required />
      <menu>
        <button class="button-primary" type="submit">Save</button>
        <button @click="setDialog(!dialogOpen)" type="button">Cancel</button>
      </menu>
    </form>
  </section>
</template>

<style scoped>
* {
  color: var(--text);
}
svg {
  color: var(--primary);
  background-color: var(--primary);
  width: 100px;
  height: 100px;
  transition: all 0.2s ease-in-out;
  background-size: 300% 100%;
  border-radius: 5px;
  box-shadow: rgba(0, 0, 0, 0.1) 0px 3px 8px;
}

svg:hover {
  cursor: pointer;
  box-shadow: rgba(0, 0, 0, 0.24) 0px 3px 8px;
  background-color: var(--primary-select);
}

.closed {
  display: none; /* Hidden by default */
}

.open {
  position: fixed;
  z-index: 1;
  left: 0;
  top: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.2);
}

form {
  background-color: var(--base-bg);
  margin: 20% auto;
  padding: 20px;
  border: 1px solid #888;
  width: 300px;
  display: flex;
  flex-direction: column;
  height: min-content;
}

menu {
  display: flex;
  justify-content: space-between;
  padding-left: 0;
  gap: 2vw;
  margin-bottom: 0;
}

menu > * {
  width: 100%;
}

button {
  all: unset;
  display: flex;
  justify-content: center;
  width: 100px;
  padding: 5px;
  padding-left: 10px;
  padding-right: 10px;
  border-radius: 5px;
  border: var(--text) solid 1px;
}

button:hover {
  cursor: pointer;
  background-color: #f1f6ec;
}

.button-primary {
  border: none;
  background-color: var(--primary);
}

.button-primary:hover {
  background-color: var(--primary-select);
}
</style>
