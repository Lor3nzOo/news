<template>
  <header
    class="md:max-w-[85rem] py-12 flex justify-center items-center flex-col mx-auto text-[#38394D] gap-6"
  >
    <nav
      class="flex-col-reverse gap-6 flex text-center md:gap-0 md:flex-row items-center justify-between mx-auto w-full"
    >
      <div
        class="flex justify-center items-center mx-auto gap-2 rounded-full px-4 py-2 border-2 border-[#38394D]"
      >
        <input
          class="outline-none text-lg"
          type="text"
          name="search"
          id="search"
          placeholder="Traži"
          v-model="query"
          @keyup.enter="fetchData(query)"
          autocomplete="off"
        />
        <button @click="fetchData(query)">
          <i class="fa-solid fa-magnifying-glass fa-xl"></i>
        </button>
      </div>
      <h1 class="font-serif text-5xl order-1 md:order-none">Glas Hrvatske</h1>
      <div class="flex justify-center items-center mx-auto gap-24 md:gap-6">
        <button class="text-lg font-semibold">Prijava</button>
        <button
          class="text-lg text-white bg-[#38394D] font-semibold px-4 py-2 rounded-full"
        >
          Pretplati Se
        </button>
      </div>
    </nav>
    <main class="md:p-5 md:max-w-none md:mb-72">
      <div
        :style="{
          'background-image': 'url(' + data.articles[0].media + ')',
        }"
        class="hidden md:max-w-none max-w-[100vw] md:flex flex-col justify-center gap-5 md:gap-72 items-start mr-auto bg-cover md:pb-0 md:pb-42 p-5 rounded-lg"
      >
        <h2
          class="font-semibold bg-blue-400 py-2 px-4 rounded-full text-xl capitalize"
        >
          {{ data.articles[0].topic }}
        </h2>
        <div
          class="flex flex-col justify-center items-start mx-auto bg-gray-100 rounded-lg w-full md:w-8/12 md:px-20 p-2 md:py-10 gap-2 md:gap-8 md:mb-[-18rem]"
        >
          <p class="font-bold opacity-75">
            {{
              `${
                data.articles[0].author == null ? "" : data.articles[0].author
              } ${data.articles[0].published_date.replaceAll("-", ".")}`
            }}
          </p>
          <h1 class="font-serif text-2xl md:text-6xl">
            {{ data.articles[0].title }}
          </h1>
          <p class="font-bold opacity-60">
            {{ data.articles[0].summary }}
          </p>
          <a
            class="text-gray-700 opacity-100 bg-gray-200 p-2"
            :href="data.articles[0].link"
            >Klikni za više</a
          >
        </div>
      </div>
      <div
        class="md:hidden border-8 border-[#38394D] p-8 flex justify-center items-start w-[95vw] flex-col gap-4 mr-auto"
      >
        <h2
          class="font-semibold bg-blue-400 py-2 px-4 rounded-full text-xl capitalize"
        >
          {{ data.articles[0].topic }}
        </h2>
        <h1 class="font-serif text-2xl">
          {{ data.articles[0].title }}
        </h1>
        <p class="font-bold opacity-75">
          {{ data.articles[0].published_date }}
        </p>
        <img
          :src="data.articles[0].media && data.articles[0].media"
          alt="Slika"
        />
        <p class="font-bold opacity-60">{{ data.articles[0].summary }}</p>
      </div>
    </main>
    <div
      class="flex flex-col justify-center items-center mx-auto gap-10 md:gap-20"
    >
      <div
        class="text-[#38394D] md:flex-row flex-col flex justify-center items-start mx-auto gap-4"
        :key="article._id"
        v-for="article in data.articles.slice(1)"
      >
        <a :href="article.link">
          <img
            class="md:max-w-[40vw] w-[90vw]"
            :src="article.media"
            alt="Slika"
          />
        </a>
        <div
          class="md:max-w-none md:w-auto w-[90vw] flex justify-center items-start mr-auto flex-col gap-2"
        >
          <h2
            class="font-semibold bg-blue-400 py-2 px-4 rounded-full text-xl capitalize"
          >
            {{ article.topic }}
          </h2>
          <h1 class="font-serif text-2xl">
            {{ article.title }}
          </h1>
          <p class="font-bold opacity-75">{{ article.summary }}</p>
          {{
            `${
              article.author == null ? "" : article.author
            } ${article.published_date.replaceAll("-", ".")}`
          }}
        </div>
      </div>
    </div>
  </header>
</template>

<script>
export default {
  name: "App",
  components: {},
  data() {
    return {
      data: [],
      query: "",
    };
  },
  methods: {
    onLoad: function () {
      fetch(
        `https://newscatcher.p.rapidapi.com/v1/latest_headlines?lang=hr&media=True&country=HR`,
        {
          method: "GET",
          headers: {
            "x-rapidapi-host": "newscatcher.p.rapidapi.com",
            "x-rapidapi-key":
              "5c110da2dfmsh4e0280cf7197e0ep1154a0jsndc93b5c7fa27",
          },
        }
      )
        .then((response) => response.json())
        .then((data) => {
          this.setData(data);
        })
        .catch((err) => console.log(err));
    },
    fetchData(q) {
      fetch(
        `https://newscatcher.p.rapidapi.com/v1/search?q=${q}&lang=hr&sort_by=relevancy&page=1&media=True&country=HR`,
        {
          method: "GET",
          headers: {
            "x-rapidapi-host": "newscatcher.p.rapidapi.com",
            "x-rapidapi-key":
              "5c110da2dfmsh4e0280cf7197e0ep1154a0jsndc93b5c7fa27",
          },
        }
      )
        .then((response) => response.json())
        .then((data) => {
          this.setData(data);
        })
        .catch((err) => console.log(err));
    },
    setData(res) {
      this.data = res;
      console.log(res);
    },
  },
  created: function () {
    this.onLoad();
  },
};
</script>
