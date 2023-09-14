<template>
  <div>
    <template v-if="module === 'vertical'">
      <UserReviewsVertical />
    </template>
    <template v-else-if="module === 'horizontal'">
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

  props: {
    mode: {
      type: String,
      default: "horizontal",
    },
  },

  mounted() {
    setTimeout(() => {
      console.log("MODE", this.mode);
    }, 1000);
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
