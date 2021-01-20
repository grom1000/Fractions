<template>
  <div class="container">
    <div class="expression">
      <div class="fraction">
        <input
          v-model="$v.leftNumerator.$model"
          type="text"
          maxlength="2"
          class="number number-numerator"
          @input="validation($v.leftNumerator.$model)"
        />
        <span class="fraction-border" />
        <input
          v-model="$v.leftDenominator.$model"
          type="text"
          maxlength="2"
          class="number number-denomunator"
          @input="validation($v.leftNumerator.$model)"
        />
      </div>
      <div class="plus alt" />
      <div class="fraction">
        <input
          v-model="$v.rightNumerator.$model"
          type="text"
          maxlength="2"
          class="number number-numerator"
          @input="validation($v.leftNumerator.$model)"
        />
        <span class="fraction-border" />
        <input
          v-model="$v.rightDenominator.$model"
          maxlength="2"
          type="text"
          class="number number-denomunator"
          @input="validation($v.leftNumerator.$model)"
        />
      </div>
      <img
        src="../assets/images/equal.png"
        alt="Знак равенства"
        class="equally"
      />
      <div v-show="!isVisible">
        {{ sum }}
      </div>
      <div v-for="(item, index) in fractions" :key="index" class="expression">
        <div class="plus alt" />
        <div class="fraction">
          <span @click="deleteFractions(index)" class="delete">&#10006;</span>
          <div>
            {{ item }}
          </div>
        </div>
      </div>
      <div v-if="isVisibleTotal" class="expression">
        <img
          src="../assets/images/equal.png"
          alt="Знак равенства"
          class="equally"
        />
        <div>{{ totalSum }}</div>
      </div>
    </div>
    <div class="content">
      <div v-if="isVisible" class="error-message">
        Введите целое число от 1 до 99
      </div>
      <button class="button" @click="addElement(1, 99)">
        add new element
      </button>
    </div>
  </div>
</template>

<script>
import { maxLength, between, numeric } from "vuelidate/lib/validators";

export default {
  name: "Fractions",
  data() {
    return {
      leftNumerator: "",
      leftDenominator: "",
      rightNumerator: "",
      rightDenominator: "",
      isVisible: false,
      isVisibleTotal: false,
      fractions: [],
      numbersAmount: 0,
      fractionsNum: 0,
      fractionsAddNum: 0
    };
  },
  validations: {
    leftNumerator: {
      maxLength: maxLength(2),
      between: between(1, 99),
      numeric
    },
    leftDenominator: {
      maxLength: maxLength(2),
      between: between(1, 99),
      numeric
    },
    rightNumerator: {
      maxLength: maxLength(2),
      between: between(1, 99),
      numeric
    },
    rightDenominator: {
      maxLength: maxLength(2),
      between: between(1, 99),
      numeric
    }
  },
  computed: {
    sum() {
      return this.leftNumerator / this.leftDenominator + this.rightNumerator / this.rightDenominator;
    },
    totalSum() {
      return this.sum + this.fractionsAddNum;
    }
  },
  methods: {
    validation() {
      if (this.$v.$anyError) {
        this.isVisible = true;
      } else {
        this.isVisible = false;
      }
    },
    addElement(min, max) {
      if (this.numbersAmount < 5) {
        let numRandom =
          Math.round(min + Math.random() * (max - min)) /
          Math.round(min + Math.random() * (max - min));
        this.fractionsNum = Number(numRandom.toFixed(2));
        this.fractions.push(this.fractionsNum);
        this.numbersAmount++;
        this.fractionsAddNum += this.fractionsNum;
        this.isVisibleTotal = true;
      }
    },
    deleteFractions(index) {
      if (this.fractions.length > 2) {
        let fractionsMinus = Number(this.fractions.splice(index, 1))
        this.numbersAmount--;
        this.fractionsAddNum -= fractionsMinus
      }
    }
  }
};
</script>
