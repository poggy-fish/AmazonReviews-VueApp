<template>
  <div class="home">
    <h2 class="title is-2">Submit Amazon Alexa Review</h2>
    <ReviewForm v-on:submit-review="submit" />
    <h2 class="title is-6 has-text-center">Recent Reviews:</h2>
    <Devices :reviews="reviews" v-if="reviews.length > 0" />
  </div>
</template>

<script>
// @ is an alias to /src
import ReviewForm from "@/components/ReviewForm.vue";
import Devices from "@/components/Devices.vue";

export default {
  name: "Home",
  props: ["reviewItems"],
  data() {
    return {
      reviews: this.reviewItems,
    };
  },
  components: {
    ReviewForm,
    Devices,
  },
  methods: {
    submit: function(formData) {
      this.$emit("submit-review", formData);
    },
  },
};

// console.log(this.reviewItems);
</script>

<style lang="sass">
@import "../styles/index.scss"
</style>
