<template>
  <div class="mydentist-app userreviews">
    <div
      id="w-node-_5ef4a456-78ee-3356-a721-49f53fdd5c23-66a39b36"
      class="userreviews-wrapper"
    >
      <h1
        id="w-node-_4bdeeea9-8aa6-fb9b-74fc-6d0ad61cef22-66a39b36"
        class="text-heading-reviews"
      >
        Omdömen från våra kunder
      </h1>
      <div class="review-block">
        <div class="review-comment">
          &quot;Vi är MyDentist - en helt ny typ av Tandvårdsklinik. Till att
          börja med ligger vi mitt i centrum i entréplan och har öppet när det
          passar dig att komma, utan vänt&quot;
        </div>
        <div class="review-name-title">Stina Nilsson</div>
        <div class="review-score-wrapper">
          <img :src="base64svg(star)" alt="" class="review-star" /><img
            :src="base64svg(star)"
            alt=""
            class="review-star"
          /><img :src="base64svg(star)" alt="" class="review-star" /><img
            :src="base64svg(star)"
            alt=""
            class="review-star"
          /><img :src="base64svg(star)" alt="" class="review-star" />
        </div>
      </div>
      <div class="review-block">
        <div class="review-comment">
          &quot;Vi är MyDentist - en helt ny typ av Tandvårdsklinik. Till att
          börja med ligger vi mitt i centrum i entréplan och har öppet när det
          passar dig att komma, utan vänt&quot;
        </div>
        <div class="review-name-title">Kent Karlsson</div>
        <div class="review-score-wrapper">
          <img :src="base64svg(star)" alt="" class="review-star" /><img
            :src="base64svg(star)"
            alt=""
            class="review-star"
          /><img :src="base64svg(star)" alt="" class="review-star" /><img
            :src="base64svg(star)"
            alt=""
            class="review-star"
          /><img :src="base64svg(star)" alt="" class="review-star" />
        </div>
      </div>
      <div class="review-block">
        <div class="review-comment">
          &quot;Vi är MyDentist - en helt ny typ av Tandvårdsklinik. Till att
          börja med ligger vi mitt i centrum i entréplan och har öppet när det
          passar dig att komma, utan vänt&quot;
        </div>
        <div class="review-name-title">Lina Andersson</div>
        <div class="review-score-wrapper">
          <img :src="base64svg(star)" alt="" class="review-star" /><img
            :src="base64svg(star)"
            alt=""
            class="review-star"
          /><img :src="base64svg(star)" alt="" class="review-star" /><img
            :src="base64svg(star)"
            alt=""
            class="review-star"
          /><img :src="base64svg(star)" alt="" class="review-star" />
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
      getJournalEntries: "get-journal-entries",
      getBookings: "bookings",
      userName: "XkehuCfMZ!hU%8h=",
      userPass: "QH5EV=2hNc*LFjJd",
      listJournalEntries: [],
      listBookings: [],
      showItemLeft: false,
      showItemRight: false,
      star: star,
    };
  },

  async created() {
    console.clear();

    this.listJournalEntries = await this.getApiData(
      this.apiBaseUrl + this.getJournalEntries
    );
    this.listBookings = await this.getApiData(
      this.apiBaseUrl + this.getBookings
    );

    // console.log(
    //   "JOURNAL ENTRIES",
    //   JSON.parse(JSON.stringify(this.listJournalEntries))
    // );
    // console.log("BOOKINGS", JSON.parse(JSON.stringify(this.listBookings)));
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

    handleTimeblockLeft(index) {
      if (this.showItemLeft === index) {
        this.showItemLeft = false;
      } else {
        this.showItemLeft = index;
      }
    },

    handleTimeblockRight(index) {
      if (this.showItemRight === index) {
        this.showItemRight = false;
      } else {
        this.showItemRight = index;
      }
    },
  },
};
</script>

<style scoped>
.time-block-content-wrapper {
  display: grid;
  grid-template-rows: 0fr;
  transition: grid-template-rows 200ms;
}

.time-block-content-wrapper.active {
  grid-template-rows: 1fr;
}

.time-block-content {
  overflow: hidden;
}
</style>
