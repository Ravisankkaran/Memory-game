<template>
  <div  id="CelebrationPage">
    <div class="container">
    <div v-for="n in 10" :key="n" class="confetti"></div>
    <div class="center">
      <h2>You won the game!</h2>
    </div>
  </div>
</div>
</template>

<script>
export default {
  name: 'CelebrationPage',
};
</script>

