<template>
  <div class="relative flex flex-row justify-center pb-8 pl-4 pr-4 pt-8">
    <div
      class="grid w-full max-w-7xl grid-cols-1 gap-4 sm:grid-cols-1 lg:w-11/12"
    >
      <div
        v-if="listReviews && listReviews.data"
        v-for="review of listReviews.data.slice(0, numberOfReviews)"
        :key="review.id"
        class="sans-serif grid-rows-auto grid grid-cols-1 gap-4 rounded-lg bg-[#ede7e2] p-6 font-fororounded sm:auto-rows-[1fr] sm:grid-cols-[1fr,auto]"
      >
        <div>
          <div
            class="pb-2.5 sm:flex sm:items-center sm:justify-start sm:pb-1.5"
          >
            <div class="text-xl font-bold md:text-2xl">
              {{ review.attributes.patient_name }}
            </div>
            <div class="flex items-center justify-start">
              <img
                :src="base64svg(checkmark)"
                alt=""
                class="mr-1.5 h-5 w-5 sm:ml-1.5"
              />
              <div class="text-left text-xs leading-4">Verifierad patient</div>
            </div>
          </div>

          <div class="pb-1.5 italic">
            &quot;{{ review.attributes.review_comment }}&quot;
          </div>
        </div>
        <div class="block sm:pl-8">
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
      <button
        v-if="!disableLoadMore"
        @click="loadMoreReviews"
        class="mt-12 box-content inline-block justify-self-center rounded-2xl border-2 border-black bg-white px-16 pb-3 pt-4 font-fororounded text-base font-semibold uppercase leading-5 text-black no-underline shadow-[inset_0_-3px_0px_-3px_#e4e4e4] transition-shadow duration-150 ease-in-out hover:shadow-[inset_0_-60px_0px_-3px_#e4e4e4]"
      >
        Ladda fler
      </button>
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
      numberOfReviews: 10,
      disableLoadMore: false,
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

    loadMoreReviews() {
      this.numberOfReviews += 7;

      if (this.numberOfReviews >= parseInt(this.listReviews.data.length)) {
        this.disableLoadMore = true;
      }
    },
  },
};
</script>
