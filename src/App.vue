<script setup>
import { onMounted, ref } from 'vue';
import DiceIcon from 'vue-material-design-icons/Dice5.vue';

const advice = ref(""),
      key = ref(Math.floor(Math.random() * 200) + 1);

const getData = async () => {
  key.value = Math.floor(Math.random() * 100) + 1; 

  try {
    const response = await fetch(`https://api.adviceslip.com/advice/${key.value}`),
          data = await response.json();

    advice.value = data.slip.advice;

  } catch(err) {
    console.error("Error fetching advice", err);
    advice.value = "Failed to get data..."
  }
}

onMounted(() => {
  getData();
});

</script>

<template>
  <div class="page">
    <div class="generator">
      <p class="generator__number">Advice #{{ key }}</p>
      <h1 class="generator__advice">{{ advice }}</h1>
      <button class="generator__button" @click="getData"> <dice-icon/> </button>
    </div>
  </div>
</template>


<style lang="scss"> 
    @import url('https://fonts.googleapis.com/css2?family=Manrope:wght@200..800&display=swap');

    $dark-blue: #202733;
    $dark-grey: #313A48;
    $grey: #4F5D74;
    $green: #52ffa8;
    $white: #fff;

    * {
       margin: 0;
    }

    .page {
      background-color: $dark-blue;
      text-align: center;
      font-family: "Manrope", sans-serif;
      height: 100vh;
      display: flex;
      align-items: center;
      justify-content: center;
    }

    .generator {
      background-color: $dark-grey;
      width: 800px;
      padding: 2rem;
      margin: 1rem;
      border-radius: 10px;
      user-select: none;

      &__number {
        color: $green;
        font-weight: 500;
        letter-spacing: 5px;
        text-transform: uppercase;
      }

      &__button {
        background-color: $green;
        border-radius: 50%;
        padding: 1rem;
        border: 0;
        cursor: pointer;
        color: #000;

        &:hover {
          background-color: darken($color: $green, $amount: 10)
        }
      }

      &__advice {
        color: $white;
        padding: 1rem;
      }
    }
</style>