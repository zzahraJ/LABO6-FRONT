<script setup>
import { ref, onMounted } from 'vue';

let socket = null;

let team = ref(null);
let score = ref(null);

// Initial teams with scores

const Score = ref([
  'De gele eendjes',
  'De blauwe eendjes',
  'De rode eendjes',
  'De groene eendjes',
]);

onMounted(() => {
    socket = new WebSocket('wss://labo6-back-a3tg.onrender.com/primus');
});

const updateScore = () => {
    socket.send(JSON.stringify({
        "action": 'updateScore', 
        "team": team.value, 
        "score": score.value,
    }));
}

</script>

<template>
    <div>
    <h1>Update Score</h1>
    <label for="teamSelector">Select your team</label>
    <select v-model="team" id="teamSelector">
        <option v-for="t in Score">
          {{ t }}
        </option>
      </select>
    <input type="number" v-model="score">
    <button type="submit" @click="updateScore">Update Score</button>
  </div>
</template>

<style scoped>


</style>