<template>
  <div class="form-container">
    <div class="item">
      Samples
    </div>
    <div class="item">
      <input class="input-item" type="number" v-model="sampleContent" />
    </div>
    <div class="item">
      <b-icon
        icon="arrow-clockwise"
        font-scale="3"
        variant="primary"
        @click="updateMarker"
      ></b-icon>
    </div>
    <div class="item">
      Radius(m)
    </div>
    <div class="item">
      <input class="input-item" type="number" v-model="radiusContent" />
    </div>
    <div>
      <b-icon
        icon="trash"
        font-scale="3"
        variant="danger"
        @click="deleteMarker"
      ></b-icon>
    </div>
  </div>
</template>

<script>
import { mapActions } from "vuex";
import { UPDATE_COORDINATE, DELETE_COORDINATE } from "../store/mutationTypes";

export default {
  name: "MyForm",
  components: {},
  data() {
    return {
      sampleContent: this.formInfo.numOfSamples,
      radiusContent: this.formInfo.radius,
      validator: /^[1-9]+[0-9]*$/
    };
  },
  props: {
    formInfo: {
      id: Number,
      radius: Number,
      numOfSamples: Number
    }
  },
  methods: {
    updateMarker() {
      if (this.simpleValidation()) {
        this.UPDATE_COORDINATE({
          id: this.formInfo.id,
          radius: parseInt(this.radiusContent),
          numOfSamples: parseInt(this.sampleContent)
        });
      } else {
        this.sampleContent = this.formInfo.numOfSamples;
        this.radiusContent = this.formInfo.radius;
      }
    },
    deleteMarker() {
      this.DELETE_COORDINATE({ id: this.formInfo.id });
    },
    simpleValidation() {
      return this.sampleValidated && this.radiusValidated;
    },
    ...mapActions([UPDATE_COORDINATE, DELETE_COORDINATE])
  },
  computed: {
    sampleValidated() {
      return this.validator.test(this.sampleContent);
    },
    radiusValidated() {
      return this.validator.test(this.radiusContent);
    }
  }
};
</script>

<style scoped lang="scss">
.form-container {
  display: grid;
  width: 100px;
  font-size: 0.5rem;
  grid-template-columns: auto 45px auto;
  grid-template-rows: auto auto;
  grid-gap: 5px;
}
.item {
  align-self: center;
}
.input-item {
  width: 45px;
}
</style>
