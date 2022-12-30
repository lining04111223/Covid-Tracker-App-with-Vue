<template>
  <main
    v-if="loading"
    class="flex flex-col align-center justify-center text-center"
  >
    <div class="text-gray-500 text-3xl mt-10 mb-6">Fetching data</div>
    <img :src="loadingImage" class="w-24 m-auto" />
  </main>
  <main v-else>
    <DataTitle :text="title" :Date="info.data.Date" />
    <data-boxes :stats="stats" />
    <country-select
      :countries="info.data.Countries"
      @getSelectedCoutryID="getCoutryByID"
    />
  </main>
</template>

<script>
import axios from "axios";
import DataTitle from "@/components/DataTitle.vue";
import DataBoxes from "@/components/DataBoxes.vue";
import CountrySelect from "@/components/CountrySelect.vue";
export default {
  name: "HomeView",
  components: {
    DataTitle,
    DataBoxes,
    CountrySelect,
  },
  data() {
    return {
      loading: true,
      title: "Global",
      stats: {},
      info: null,
      errored: false,
      loadingImage: require("../assets/kOnzy.gif"),
    };
  },
  methods: {
    getCoutryByID(sendedID) {
      let result = "";
      if (sendedID === "1") {
        this.stats = this.info.data.Global;
        this.title = "Global";
      } else {
        result = this.info.data.Countries.find((item) => item.ID === sendedID);
        console.log("this", result);
        this.stats = result;
        this.title = result.Country;
      }
    },
  },
  mounted() {
    axios
      .get("https://api.covid19api.com/summary")
      .then((response) => {
        this.info = response;
        console.log("globle", this.info.data.Global);
        this.stats = response.data.Global;
      })
      .catch((error) => {
        console.log(error);
        this.errored = true;
      })
      .finally(() => (this.loading = false));
  },
};
</script>
