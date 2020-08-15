<template>
  <b-carousel-list
    class="carousel"
    :data="reviewItems"
    :items-to-show="3"
    :images="images"
    v-if="reviewItems.length > 0"
  >
    <template slot="item" slot-scope="props" class="carousel-slide">
      <div class="card">
        <div class="card-image">
          <figure class="image is-128x128">
            <a @click="info(props.index)">
              <img :src="images[props.list.deviceVariant]" />
            </a>
          </figure>
        </div>
        <div class="card-content">
          <div class="content">
            <p class="title is-7">
              Device: {{ props.list.deviceVariant }} Alexa
            </p>
            <p class="subtitle is-size-7">Customer: {{ props.list.name }}</p>
            <div class="field is-grouped">
              <p class="control" v-if="props.list.rating">
                <b-rate :value="props.list.rating" show-score disabled />
              </p>
            </div>
            <p class="has-text-center is-size-7">
              <b>Review:</b> {{ props.list.message }}
            </p>
            <p class="timestamp is-size-7 has-text-center">
              Timestamp: {{ props.list.timestamp }}
            </p>
          </div>
        </div>
      </div>
    </template>
  </b-carousel-list>
</template>

<script>
// Device Images
import charcoal from "@/assets/charcoal_fabric.jpg";
import sandstone from "@/assets/charcoal_fabric.jpg";
import black from "@/assets/black.png";
import white from "@/assets/white.jpg";
import walnut from "@/assets/walnut_finish.jpg";
import heather from "@/assets/heather_gray_fabric.jpg";
import oak from "@/assets/oak_finish.jpg";

export default {
  name: "Devices",
  props: ["reviews"],
  data() {
    return {
      reviewItems: this.reviews,
      images: {
        "Charcoal Fabric": charcoal,
        "Sandstone Fabric": sandstone,
        Black: black,
        White: white,
        "Walnut Finish": walnut,
        "Heather Gray Fabric": heather,
        "Oak Finish": oak,
      },
    };
  },
  methods: {
    info(value) {
      this.test = value;
    },
  },
};
</script>
<style lang="scss" scoped>
@import "../styles/_devices.scss";
</style>
