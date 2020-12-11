<template>
  <main class="p-10">
    <form @submit.prevent="search">
      <div class="flex justify-center m-auto">
        <input
          v-model="searchTerm"
          type="text"
          placeholder="Search for movie..."
          class="rounded-md lg:w-2/12 bg-gray-100 pl-4 border-2 border-black text-black"
        />
        <button
          @click="search"
          class="bg-yellow-300 p-4 pt-2 pb-2 text-2 rounded-md ml-4 text-dark uppercase"
        >
          Search
        </button>
      </div>
    </form>
    <div class="flex flex-wrap justify-around">
      <div
        v-for="img of imgs"
        :key="img.title"
        class="w-64 bg-blue-500 p-2 rounded-md mt-5"
      >
        <img :src="img.poster" alt="" />
        <h3 class="text-xl font-extrabold">{{ img.title }}</h3>
        <span>{{ img.date }}</span>
      </div>
    </div>
  </main>
</template>

<script>
const popURL =
  "https://api.themoviedb.org/3/discover/movie?sort_by=popularity.desc&api_key=04c35731a5ee918f014970082a0088b1&page=1";
const bg = "https://image.tmdb.org/t/p/w1280";
const searchURL =
  "https://api.themoviedb.org/3/search/movie?&api_key=04c35731a5ee918f014970082a0088b1&query=";
const imgTest =
  "https://image.tmdb.org/t/p/w1280/ugZW8ocsrfgI95pnQ7wrmKDxIe.jpg";
export default {
  data: () => ({
    imgTest: imgTest,
    imgs: [],
    searchTerm: "",
  }),
  methods: {
    search() {
      if (this.searchTerm === "") {
        return;
      }
      fetch(`${searchURL}${this.searchTerm}`)
        .then((val) => val.json())
        .then((movies) => {
          console.log("movies", movies);
          this.imgs = this.parseImgResponse(movies);
        });
    },
    parseImgResponse(movies) {
      return movies.results.reduce((acc, movie) => {
        if (!movie.poster_path) {
          return acc;
        }
        acc.push({
          poster: `${bg}${movie.poster_path}`,
          date: movie.release_date,
          title: movie.title,
        });
        return acc;
      }, []);
    },
  },
  mounted() {
    fetch(popURL)
      .then((info) => info.json())
      .then((movies) => {
        console.log(movies);
        this.imgs = this.parseImgResponse(movies);
      });
  },
};
</script>

<style>
/* Sample `apply` at-rules with Tailwind CSS
.container {
@apply min-h-screen flex justify-center items-center text-center mx-auto;
}
*/
input::placeholder {
  color: black;
}
</style>