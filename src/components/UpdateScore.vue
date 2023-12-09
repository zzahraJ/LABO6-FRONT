<script setup>
import { ref, onMounted } from 'vue';

let socket = null;

// Initial teams with scores

const teams = ref([
  {team: 'De gele eendjes', score: 0},
  {team: 'De blauwe eendjes', score: 0},
  {team: 'De rode eendjes', score: 0},
  {team: 'De groene eendjes', score: 0},
]);

onMounted(() => {
    socket = new WebSocket('wss://labo6-back-a3tg.onrender.com/primus');
});

const updateScore = (team, score) => {
    socket.send(JSON.stringify({
        action: 'updateScore', 
        team: team, 
        score: score
    }));
}

</script>

<template>
    <div>
    <h1>Update Score</h1>
    <label for="teamSelector">Select your team</label>
    <select v-model="team" id="teamSelector">
        <option v-for="t in teamScores">
          {{ t }}
        </option>
      </select>
    <input type="number" v-model="score">
    <button type="submit" @click="updateScore">Update Score</button>
  </div>
</template>

<style scoped>


</style>