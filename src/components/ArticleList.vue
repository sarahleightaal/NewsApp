<template>
  <div v-if="errored">
    <h1>
      An error has occured <br />
      Please try again later
    </h1>
  </div>
  <div v-if="loading">Loading...</div>

  <div class="listContainer">
    <ul>
      <li v-for="article in articles" :key="article.id">
        <img v-bind:src="article.urlToImage" alt="" />

        <div class="textContent">
          <h3>{{ article.author }}</h3>

          <p>{{ article.content }}</p>
        </div>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  props: ["search", "country"],
  data() {
    return {
      articles: [],
      loading: true,
      errored: false,
    };
  },
  mounted() {
    this.getArticles();
  },
  methods: {
    getArticles() {
      const api = "https://newsapi.org/v2/top-headlines";
      const params = {
        apiKey: "d36d9da7ad2746268e11edc18bc5e445",
      };
      if (this.search) {
        params.q = this.search;
      }
      if (this.country) {
        params.country = this.country.toLowerCase();
      }
      this.axios
        .get(api, { params })
        .then((response) => {
          this.articles = response.data.articles;
        })
        .catch((error) => {
          console.log(error);
          this.errored = true;
        })
        .finally(() => {
          this.loading = false;
        });
    },
  },
  watch: {
    search() {
      this.getArticles();
    },
    country() {
      this.getArticles();
    },
  },
};
</script>

<style scoped>
p,
h3,
ul {
  margin: 0;
  padding: 10px;
  list-style-type: none;
}

img {
  width: 150px;
  position: cover;
}

.listContainer {
  background-color: rgb(3, 30, 70, 0.7);
}

.listContainer li {
  list-style-type: none;
  background-color: rgb(251, 233, 197);
  color: rgb(3, 30, 70);
  margin: 20px auto;
  padding: 10px 20px;
  border-radius: 10px;
  display: flex;
  align-items: center;
}
</style>
