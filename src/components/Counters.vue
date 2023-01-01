<template>
  <div>
    <div class="counter-container">
      <div class="counter">
        <p>
          <font-awesome-icon icon="fa-solid fa-thumbs-up"/>
          {{ correctAnswers }}/{{ totalQuestions }}
        </p>
      </div>
      <div class="counter">
        <p>
          <font-awesome-icon icon="fa-solid fa-thumbs-down"/>
          {{ incorrectAnswers }}/{{ totalQuestions }}
        </p>
      </div>
    </div>
  </div>
</template>


<style>

</style>


<script>

</script>