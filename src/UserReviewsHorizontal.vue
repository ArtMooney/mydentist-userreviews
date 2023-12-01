<template>
  <div class="relative flex flex-col items-center pb-8 pl-4 pr-4 pt-8">
    <div
      class="grid w-full max-w-7xl grid-cols-1 gap-4 sm:grid-cols-1 md:grid-cols-3 lg:w-11/12"
    >
      <div
        v-if="listReviews && listReviews.data"
        v-for="review of listReviews.data.slice(0, 3)"
        :key="review.id"
        class="sans-serif flex min-h-[12rem] flex-col items-start justify-around rounded-lg bg-[#ccbcaa] p-6 font-fororounded sm:min-h-[14rem] lg:min-h-[16rem]"
      >
        <div class="pb-4 italic leading-5">
          &quot;{{ review.attributes.review_comment }}&quot;
        </div>
        <div>
          <div class="text-xl font-bold">
            {{ review.attributes.patient_name }}
          </div>
          <div class="flex items-center justify-start">
            <img
              :src="base64svg(checkmark)"
              alt=""
              class="ml-0 mr-1.5 h-5 w-5"
            />
            <div class="text-left text-xs leading-4">Verifierad patient</div>
          </div>
          <div class="mb-4 mt-4 flex">
            <img
              v-for="(item, index) in getRepeatTimes(
                review.attributes.overall_rating,
              )"
              :src="base64svg(star)"
              alt=""
              class="inline-block w-4"
            />
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import star from "./images/star.svg?raw";
import checkmark from "./images/checkmark.svg?raw";

export default {
  name: "UserReviews",

  data() {
    return {
      apiBaseUrl: "https://api.ngine.se/webhook/mydentist/",
      getReviews: "get-reviews",
      userName: "XkehuCfMZ!hU%8h=",
      userPass: "QH5EV=2hNc*LFjJd",
      listReviews: [],
      star: star,
      checkmark: checkmark,
    };
  },

  async created() {
    this.listReviews = await this.getApiData(this.apiBaseUrl + this.getReviews);
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
