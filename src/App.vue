<template>
  <div id="app">
    <Navbar id="nav" />
    <router-view v-on:submit-review="submit" :reviewItems="reviewItems" />
    <Footer />
    <b-loading :is-full-page="true" :active.sync="isLoading" :can-cancel="true">
      <b-icon pack="fas" icon="sync-alt" size="is-large" custom-class="fa-spin"></b-icon>
    </b-loading>
  </div>
</template>

<script>
// @ is an alias to /src
import Navbar from "@/components/Navbar.vue";
import Footer from "@/components/Footer.vue";
import moment from "moment";
// Firebase Import
import Firebase from "firebase/app";
import "firebase/database";
import "firebase/storage";

// Your web app's Firebase configuration
const firebaseConfig = {
    apiKey: "AIzaSyBC4axQWtJGKG6cXeaQidcPzvg33U_Thcs",
    authDomain: "ig-demp.firebaseapp.com",
    databaseURL: "https://ig-demp-default-rtdb.firebaseio.com",
    projectId: "ig-demp",
    storageBucket: "ig-demp.appspot.com",
    messagingSenderId: "444607976730",
    appId: "1:444607976730:web:34a5aa01aa049970e948a3"
};

// Initialize Firebase
Firebase.initializeApp(firebaseConfig);
let db = Firebase.database();
let reviewsRef = db.ref("reviews");

export default {
  name: "reviewApp",
  data() {
    return {
      isLoading: true,
      reviewItems: [],
    };
  },
  created() {
    this.isLoading = true;
  },
  mounted() {
    // Gets Review Data
    this.getData();
  },
  watch: {
    // Whenever reviewItems changes, this function will run
    // Turns off Load state when reviewItems are populated
    reviewItems: function () {
      this.isLoading = false;
    },
  },
  components: {
    Navbar,
    Footer,
  },
  methods: {
    submit: async function (formData) {
      // Boolean
      let submitted = false;
      // Sends formData to Firebase Realtime Database
      await reviewsRef.push(
        { ...formData, timestamp: moment().format() },
        (error) => {
          if (error) {
            // The write failed... Open buefy toast with error
            this.$buefy.toast.open({
              duration: 3000,
              message: `Error Submitting Form: ${error}`,
              position: "is-top",
              type: "is-danger",
            });
          } else {
            // Data saved successfully! Open buefy toast with success
            this.$buefy.toast.open({
              duration: 3000,
              message: `Successfully Submitted Review`,
              position: "is-top",
              type: "is-success",
            });
            // Submission success
            submitted = true;
          }
        }
      );

      // Gets Updated Review Data w/ new Review
      if (submitted) {
        this.getData();
      }
    },
    getData: function () {
      // Reset Reviews Array
      this.reviewItems.length = 0;
      // Retrieves reviews from Firebase
      reviewsRef.on("value", (reviews) => {
        reviews.forEach((review) => {
          this.reviewItems.unshift({
            deviceVariant: review.child("deviceVariant").val(),
            message: review.child("message").val(),
            name: review.child("name").val(),
            rating: review.child("rating").val(),
            timestamp: review.child("timestamp").val(),
          });
        });
      });
    },
  },
};
</script>

