<template>
  <section class="second-step">
    <h1>Choose the printing...</h1>
    <div class="preview">
      <div>
        <h3>Printing:</h3>
        <img :src="currentImg" />
        <div class="preview-buttons">
          <button @click.prevent="handleGenerateNewImg">
            Generate new printing
          </button>
          <button @click.prevent="handleGoBackward" v-if="previousImg">
            Load previous printing
          </button>
          <button
            @click.prevent="handleGoForward"
            v-if="currentImg !== randomImg"
          >
            Go one forward
          </button>
        </div>
      </div>
      <div>
        <h3>T-shirt preview:</h3>
        <TshirtPreview :url="currentImg" :isFront="isFront" :isBack="isBack" />
      </div>
    </div>
    <p>Price: {{ price }}</p>
    <button class="btn btn-prev" @click="onPrevStepChange">
      Previous step
    </button>
    <button class="btn btn-next" @click="onNextStepChange">Next step</button>
  </section>
</template>

<script lang="ts">
import TshirtPreview from "@/components/TshirtPreview/TshirtPreview.vue";
import Vue, { PropType } from "vue";
import { FormType } from "../FifthStep/FifthStep.utils";

export default Vue.extend({
  name: "FormSecondStepComponent",
  components: {
    TshirtPreview,
  },
  props: {
    isFront: Boolean,
    isBack: Boolean,
    price: Number,
    randomImg: String,
    handleRefetch: Function,
    form: Object as PropType<FormType>,
  },
  data() {
    return {
      currentImg: this.randomImg,
      previousImg: "",
    };
  },

  watch: {
    randomImg(newVal: string) {
      this.currentImg = newVal;
    },
  },

  methods: {
    handleGenerateNewImg() {
      this.previousImg = this.currentImg;
      this.handleRefetch();
      this.currentImg = this.randomImg;
    },
    handleGoBackward() {
      if (this.previousImg) {
        this.currentImg = this.previousImg;
        this.previousImg = "";
      }
    },
    handleGoForward() {
      if (this.currentImg !== this.randomImg) {
        this.previousImg = this.currentImg;
        this.currentImg = this.randomImg;
      }
    },
    onPrevStepChange() {
      this.$emit("update:prevStep");
    },

    onNextStepChange() {
      this.$emit("update:nextStep");
    },
  },
});
</script>

<style scoped lang="scss">
.second-step {
  h1 {
    color: #333;
    font-size: 48px;
    margin-bottom: 1rem;
  }

  h3 {
    color: #333;
    font-weight: 500;
    margin-bottom: 0.5rem;
  }

  p {
    font-size: 18px;
    color: #333;
    font-weight: 500;
    margin-bottom: 1rem;
  }
}

.preview {
  display: flex;
  gap: 4rem;

  & > div {
    img {
      margin-bottom: 1rem;
    }
  }
}

.preview-buttons {
  display: flex;
  flex-direction: column;
  gap: 0.5rem;

  button {
    font-weight: bold;
    color: white;
    border-color: transparent;
    padding: 0.1rem 0.3rem;
    width: 160px;
    cursor: pointer;
    border-radius: 2px;
  }

  button:nth-of-type(1) {
    background-color: #ec9b20;
  }
  button:nth-of-type(2) {
    background-color: crimson;
  }
}
</style>
