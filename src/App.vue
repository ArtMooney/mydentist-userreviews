<style>
@import "./css/normalize.css";
@import "./css/components.css";
@import "./css/mydentist-apps.css";
</style>

<template>
  <UserReviews />
</template>

<script>
import UserReviews from "./UserReviews.vue";

export default {
  name: "App",
  components: { UserReviews },
};
</script>
