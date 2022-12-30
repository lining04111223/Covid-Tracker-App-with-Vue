<template>
  <main
    v-if="loading"
    class="flex flex-col align-center justify-center text-center"
  >
    <div class="text-gray-500 text-3xl mt-10 mb-6">Fetching data</div>
    <img :src="loadingImage" class="w-24 m-auto" />
  </main>
  <main v-else><DataTitle :text="title" :Date="info.data.Date" /></main>
</template>

<script>
import axios from "axios";
import DataTitle from "@/components/DataTitle.vue";

export default {
  name: "HomeView",
  components: {
    DataTitle,
  },
  data() {
    return {
      loading: true,
      title: "Global",
      info: null,
      errored: false,
      loadingImage: require("../assets/kOnzy.gif"),
    };
  },
  created() {
    axios
      .get("https://api.covid19api.com/summary")
      .then((response) => {
        this.info = response;
        console.log(this.info.data.Date);
      })
      .catch((error) => {
        console.log(error);
        this.errored = true;
      })
      .finally(() => (this.loading = false));
  },
};
</script>
