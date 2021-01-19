<template>
  <div class="container">
    <div class="expression">
      <div class="fraction">
        <input
          v-model="leftNumerator"
          type="text"
          maxlength="2"
          class="number number-numerator"
          @input="handleValidation(leftNumerator)"
        >
        <span class="fraction-border" />
        <input
          v-model="leftDenominator"
          type="text"
          maxlength="2"
          class="number number-denomunator"
          @input="handleValidation(leftDenominator)"
        >
      </div>
      <div class="plus alt" />
      <div class="fraction">
        <input
          v-model="rightNumerator"
          type="text"
          maxlength="2"
          class="number number-numerator"
          @input="handleValidation(rightNumerator)"
        >
        <span class="fraction-border" />
        <input
          v-model="rightDenominator"
          maxlength="2"
          type="text"
          class="number number-denomunator"
          @input="handleValidation(rightDenominator)"
        >
      </div>
      <img src="../assets/images/equal.png" alt="Знак равенства" class="equally">
      <div v-show="!isVisible">
        {{ sum }}
      </div>
    </div>
    <div v-if="isVisible">
      Введите целое число от 1 до 99
    </div>
  </div>
</template>

<script>
export default {
  name: 'Fractions',
  data() {
    return {
      leftNumerator: '',
      leftDenominator: '',
      rightNumerator: '',
      rightDenominator: '',
      isVisible: false
    }
  },
  computed: {
    sum() {
      return this.leftNumerator / this.leftDenominator + this.rightNumerator / this.rightDenominator
    }
  },
  methods: {
    handleValidation(val) {
      const numbers = /^[-+]?[0-9]+$/;
      if(val.match(numbers) && val != 0) {
        this.isVisible = false
      } else {
        this.isVisible = true
      }
    }
  }
}
</script>
