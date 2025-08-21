<template>

  <div class="crt-effect">
    <div class="scanlines"></div>
    <div class="container">
      <header>
        <h1>METAL SLUG X</h1>
        <div class="player-info">
          <input ref="refNameInput" v-if="!playerName" v-model="playerTagInput" @keyup.enter="setPlayerName"
            placeholder="ENTER PLAYER NAME" type="text" maxlength="12">
          <button type="button" v-if="!playerName" @click="setPlayerName">INSERT COIN</button>
          <div v-if="playerName" class="player-tag">
            PLAYER: {{ playerName }}
            <button type="button" class="remove-btn" @click="clearPlayerName">
              <i class="fas fa-times"></i> CHANGE
            </button>
          </div>
        </div>
      </header>

      <div class="instructions">
        <p>SELECT YOUR CHARACTER - PRESS START TO BEGIN MISSION</p>
      </div>

      <div class="character-grid">
        <div v-for="(character, index) in characters" :key="index"
          :class="['character-card', { selected: character.selected }]" @click="selectCharacter(character.id)">
          <div class="character-image">
            <img :src="character.gif" :alt="character.name">
          </div>
          <div class="character-info">
            <h3 class="character-name">{{ character.name }}</h3>
            <p>{{ character.description }}</p>
            <div class="character-stats">
              <div class="stat">
                <div class="stat-value">{{ character.health }}</div>
                <div class="stat-label">HEALTH</div>
              </div>
              <div class="stat">
                <div class="stat-value">{{ character.speed }}</div>
                <div class="stat-label">SPEED</div>
              </div>
              <div class="stat">
                <div class="stat-value">{{ character.power }}</div>
                <div class="stat-label">POWER</div>
              </div>
            </div>
          </div>
          <div v-if="character.selected" class="selection-badge">SELECTED</div>
        </div>
      </div>

      <footer>
        <p>© 2023 SNK CORPORATION - METAL SLUG ARCADE EDITION</p>
      </footer>
    </div>
  </div>

</template>


<script setup>
import { ref, onMounted } from 'vue';
import data from '@/data/characters.json'


const playerTagInput = ref("");
const playerName = ref("");
const selectedCharacterId = ref(null);

const characters = ref(data);

const refNameInput = ref(null)

onMounted(()=>{
  refNameInput.value.focus()
})


const setPlayerName = () => {
  if (playerTagInput.value.trim()) {
    playerName.value = playerTagInput.value.toUpperCase();
    playerTagInput.value = "";
  }
};

const clearPlayerName = () => {
  playerName.value = "";
  selectedCharacterId.value = null;
  characters.value.forEach((char) => (char.selected = false));
};

const selectCharacter = (charId) => {
  if (!playerName.value) {
    alert("Please enter your name first")
    return;
  }

  characters.value.forEach((char) => {
    char.selected = char.id === charId;
  });

  selectedCharacterId.value = charId;
};



</script>
