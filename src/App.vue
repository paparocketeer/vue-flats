<template>
  <div id="app">
    <div class="container">
      <div class="header">
        <h1>Flats test app</h1>
      </div>
      <div class="flats" v-if="!loading">
        <Flat
          v-for="flat in flats"
          :key="flat.id"
          :flat="flat"
          v-on:flat-fav-toggled="flatToggled"
        />
      </div>
      <div v-else>
        <Spinner />
      </div>
    </div>
  </div>
</template>

<script>
import Flat from "@/components/Flat";
import Spinner from "@/components/Spinner";
export default {
  name: "App",
  components: {
    Flat,
    Spinner,
  },
  data() {
    return {
      flats: [],
      loading: true,
    };
  },
  created() {
    // fetching favs from localstorage
    let favs = localStorage["favs"] ? JSON.parse(localStorage["favs"]) : [];
    // fetching flats from fake api and setting favs on them while putting inside data object
    fetch(process.env.VUE_APP_ROOT_API)
      .then((resp) => resp.json())
      .then((json) => {
        json.forEach((flat) => {
          if (favs.includes(flat.id)) {
            flat.fav = true;
          }
          this.flats.push(flat);
        });
      });

    // fake fetching time
    setTimeout(() => {
      this.loading = false;
    }, 600);
  },
  methods: {
    flatToggled(id, fav) {
      let favs = localStorage["favs"] ? JSON.parse(localStorage["favs"]) : [];
      if (fav) {
        favs.push(id);
      } else {
        favs = favs.filter((item) => item !== id);
      }
      localStorage.setItem("favs", JSON.stringify(favs));
    },
  },
};
</script>

<style lang="scss">
@import "assets/scss/main";
</style>
