<template>
  <div class="mydentist-app userreviews vertical">
    <div
      id="w-node-_5ef4a456-78ee-3356-a721-49f53fdd5c23-ff84f577"
      class="userreviews-wrapper vertical"
    >
      <div
        v-if="listReviews && listReviews.data"
        v-for="review of listReviews.data.slice(0, 7)"
        :key="review.id"
        class="review-block vertical"
      >
        <div>
          <div class="review-name-title">
            {{ review.attributes.patient_name }}
          </div>
          <div class="review-comment">
            &quot;{{ review.attributes.review_comment }}&quot;
          </div>
        </div>
        <div
          id="w-node-_43e78301-f4e5-c172-1fbb-c71ce5cf950c-ff84f577"
          class="review-score-wrapper vertical"
        >
          <img
            v-for="(item, index) in getRepeatTimes(
              review.attributes.overall_rating
            )"
            :src="base64svg(star)"
            alt=""
            class="review-star"
          />
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import star from "./images/star.svg?raw";

export default {
  name: "UserReviews",

  data() {
    return {
      apiBaseUrl: "https://api.ngine.se/webhook/mydentist/",
      getReviews: "get-reviews",
      userName: "XkehuCfMZ!hU%8h=",
      userPass: "QH5EV=2hNc*LFjJd",
      listReviews: [],
      showItemLeft: false,
      showItemRight: false,
      star: star,
    };
  },

  async created() {
    console.clear();

    this.listReviews = await this.getApiData(this.apiBaseUrl + this.getReviews);

    console.log("REVIEWS", JSON.parse(JSON.stringify(this.listReviews)));
  },

  methods: {
    getApiData(urlEndpoint) {
      return new Promise((resolve, reject) => {
        var requestOptions = {
          method: "GET",
          headers: {
            Authorization: "Basic " + btoa(this.userName + ":" + this.userPass),
          },
          redirect: "follow",
        };

        fetch(urlEndpoint, requestOptions)
          .then((response) => {
            if (!response.ok) throw new Error();
            return response.json();
          })
          .then((result) => {
            resolve(result);
          })
          .catch((error) => {
            console.log(error);

            reject(error);
          });
      });
    },

    base64svg(image) {
      return `data:image/svg+xml;base64,${btoa(image)}`;
    },

    formattedDate(dateString) {
      const date = new Date(dateString);
      const options = { year: "numeric", month: "long", day: "numeric" };
      return date.toLocaleDateString(undefined, options);
    },

    getRepeatTimes(times) {
      return [...Array(times).keys()];
    },
  },
};
</script>