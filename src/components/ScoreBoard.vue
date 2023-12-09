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

socket.onmessage = (event) => {
  const score = JSON.parse(event.data);
  console.log(score);
  if (score.action === 'updateScore') {
    // Check if the team is one of the default teams
    let teamSelected = teams.value.findIndex(
      (t) => t.team === score.team
    );
    if (teamSelected !== -1) {
      // Update the score for the default team
      teams.value[teamSelected].score = score.score;
    }
  }
};
});

</script>

<template>
   <div>
    <ul>
        <li v-for="t in teams" :key="t.team">
        {{ t.team }}: {{ t.score }}
      </li>  
    </ul>
  </div>
</template>

<style scoped>

ul{
  list-style-type: none;
  margin: 10;
  padding: 10;
  overflow: hidden;   
}
li{
  float: left;
  padding: 10px;
  margin: 10px;
  background-color: #f2f2f2;
  border-radius: 5px;
  border: 1px solid #ccc;
  font-family: Arial, Helvetica, sans-serif;
}


</style>
