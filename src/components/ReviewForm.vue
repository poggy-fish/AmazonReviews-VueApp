<template>
  <section class="form">
    <!-- Name Field -->
    <b-field
      label="Name"
      name="name"
      v-bind:type="formErrors.name ? 'is-danger' : ''"
      v-bind:message="formErrors.name ? 'Name Cannot Be Empty' : ''"
    >
      <b-input
        v-model="formData.name"
        v-on:input="(e)=> handleInput(e, 'name')"
        placeholder="Enter your name..."
      ></b-input>
    </b-field>

    <!-- Select Device Field -->
    <b-field
      label="Select Device"
      v-bind:type="formErrors.deviceVariant ? 'is-danger' : ''"
      v-bind:message="
        formErrors.deviceVariant ? 'Device Variant Cannot Be Empty' : ''
      "
    >
      <b-select
        v-model="formData.deviceVariant"
        v-on:input="(e)=> handleInput(e, 'deviceVariant')"
        expanded
      >
        <option disabled value>Select Device Variant</option>
        <option v-for="device in options" v-bind:key="device" v-bind:value="device">{{ device }}</option>
      </b-select>
    </b-field>

    <!-- Review Message Field -->
    <b-field
      label="Review"
      v-bind:type="formErrors.message ? 'is-danger' : ''"
      v-bind:message="formErrors.message ? 'Review Message Cannot Be Empty' : ''"
    >
      <b-input
        v-model="formData.message"
        maxlength="500"
        type="textarea"
        placeholder="Write Review..."
        v-on:input="(e)=> handleInput(e, 'message')"
      ></b-input>
    </b-field>

    <!-- Rating Stars Block -->
    <div class="block">
      <h3 class="title">Rating:</h3>
      <div class="starContainer">
        <div
          class="star"
          v-bind:id="`star_${star}`"
          v-on:click="rate"
          v-for="star in stars"
          v-bind:key="star"
        >
          <b-radio-button
            class="check"
            type="radio"
            v-bind:native-value="star"
            v-model="formData.rating"
            v-on:input="(e)=> handleInput(e, 'rating')"
          />
          <label v-bind:for="star">
            <b-icon icon="star" size="is-large"></b-icon>
          </label>
        </div>
      </div>
    </div>
    <b-field v-if="formErrors.rating" type="is-danger" message="Rating cannot be empty"></b-field>
    <!-- <Upload /> -->
    <b-button v-on:click="submit" icon-left="check" size="is-medium" expanded>Submit</b-button>
  </section>
</template>

<script>
import Firebase from "firebase/app";
let db = Firebase.database();
let deviceVariants = db.ref("deviceVariants");

export default {
  data() {
    return {
      stars: [5, 4, 3, 2, 1],
      formData: {
        name: "",
        deviceVariant: "",
        message: "",
        rating: "",
      },
      formErrors: {
        name: false,
        deviceVariant: false,
        message: false,
        rating: false,
      },
      options: [],
      validCount: 0,
    };
  },
  mounted() {
    // Fetch New Options from Firebase
    this.options.length = 0;

    deviceVariants.on("value", (variants) => {
      this.options = variants.val();
    });
  },
  beforeDestroy() {
    deviceVariants.off();
  },
  methods: {
    // Method to submit form
    submit() {
      // Form Validation Loop
      for (const key in this.formData) {
        // If key is empty then throw error
        if (this.formData[key] === "") {
          this.formErrors[key] = true;
          // Open buefy toast with error
          let keyName = key.charAt(0).toUpperCase() + key.slice(1);
          this.$buefy.toast.open({
            duration: 2000,
            message: `${keyName} cannot be empty`,
            position: "is-top",
            type: "is-danger",
          });
          // Key is not empty toggle error off
        } else {
          this.formErrors[key] = false;
          this.validCount += 1;
        }
      }

      // Check that all fields have been validated
      if (this.validCount === Object.keys(this.formData).length) {
        // Send form data up to parent component
        this.$emit("submit-review", this.formData);

        // Reset Form Data
        this.formData = {
          name: "",
          deviceVariant: "",
          message: "",
          rating: "",
        };
        // Reset Rating Stars
        this.resetRating();
      }

      // Update valid count and loading state
      this.validCount = 0;
    },
    // Input change handlers to toggle form errors
    handleInput(val, type) {
      val !== ""
        ? (this.formErrors[type] = false)
        : (this.formErrors[type] = true);
    },
    // Resets Stars Selected Status
    resetRating() {
      let stars = [...document.getElementsByClassName("star")];
      // Remove selected class from stars
      stars.forEach((element) => {
        if (element.className === "star selected") {
          element.classList.remove("selected");
        }
      });
    },
    // Method to select device rating
    rate(e) {
      // Variable Initialization
      const value = e.target.firstElementChild.value;
      let stars = [...document.getElementsByClassName("star")];
      let rating = document.getElementById(`star_${value}`);

      // Toggle rating selected class
      rating.classList.toggle("selected");

      // Check if other ratings have already been selected and remove selected class
      stars.forEach((element) => {
        if (element.className === "star selected" && element !== rating) {
          element.classList.remove("selected");
        }
      });
    },
  },
};
</script>
