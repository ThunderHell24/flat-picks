<template>
  <span class="separator">|</span>
  <div class="filter-range">
    <h3 class="filter-name">{{ slide.name }}</h3>
    <div class="filter-range_values">
      <input
        type="number"
        v-model="valueProp[0]"
        @change="setValues(valueProp, slide.filterName)"
        size="2"
        min="slide.value[0]"
      />
      <span class="hyphen">-</span>
      <input
        type="number"
        v-model="valueProp[1]"
        @change="setValues(valueProp, slide.filterName)"
        size="2"
        max="slide.value[1]"
      />
    </div>

    <vue-slider
      ref="slider"
      :min="min"
      :max="max"
      @mouseup.prevent="setValues(valueProp, slide.filterName)"
      v-model="valueProp"
      v-bind="options"
    />
  </div>
</template>

<script lang="ts">
import { computed } from "vue";
import { mapActions, mapGetters, mapMutations } from "vuex";
import VueSlider from "vue-slider-component";
import "../sass/slider.css";

export default {
  name: "Sliders",
  props: { slide: Object },
  data() {
    return {
      valueProp: this.slide.value,
      minFloor: this.slide.value[0],
      maxFloor: this.slide.value[1],
      options: {
        width: "100%",
        height: 4,
        tooltip: "none",
        min: this.slide.value[0],
        max: this.slide.value[1],
        minRange: this.slide.value[0],
        maxRange: this.slide.value[1],
        interval: this.slide.step
      }
    };
  },
  components: { VueSlider },
  methods: {
    setValues(val: any, name: any): any {
      const min = val[0];
      const max = val[1];
      const arr = [min, max, name];
      this.setFilter(arr);
    },
    ...mapGetters(["allFlats", "filteredFlats"]),
    ...mapActions(["setFilter"])
  },
  computed: {}
};
</script>

<style lang="sass">
.vue-slider
  margin: 1rem
  margin-right: 1.5rem
</style>
