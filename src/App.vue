<template>
  <div>
    <template v-if="mode === 'vertical'">
      <UserReviewsVertical />
    </template>
    <template v-else-if="mode === 'horizontal'">
      <UserReviewsHorizontal />
    </template>
  </div>
</template>

<script>
import UserReviewsHorizontal from "./UserReviewsHorizontal.vue";
import UserReviewsVertical from "./UserReviewsVertical.vue";

export default {
  name: "App",
  components: {
    UserReviewsHorizontal,
    UserReviewsVertical,
  },

  data() {
    return {
      mode: "",
    };
  },

  created() {
    this.mode = localStorage.getItem("userreviewsMode") || "horizontal";
  },

  mounted() {
    localStorage.removeItem("userreviewsMode");
  },

  computed: {
    module() {
      const url = window.location.href;
      const query = url.split("?")[1];

      if (query === "horizontal" || query === "vertical") {
        return query;
      } else {
        return "horizontal";
      }
    },
  },
};
</script>
