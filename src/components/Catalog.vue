<template>
  <div class="wrapper">
    <div class="title-request">
      <span>Resuts for</span>
      <h2>{{ titleRequest }}</h2>
    </div>
    <div class="related f--size-sm">
      <div class="text--grey related__title">Explore titles related to:</div>
      <ul class="related__list">
        <li
          class="list__item f--size-sm"
          v-for="(item, i) in filterBy()"
          :key="`title_${i}`"
        >
          <a :href="`#film_${i}`">{{ item.nameTitle }}</a>
        </li>
      </ul>
    </div>
    <ul class="catalog">
      <li
        class="catalog__item"
        v-for="(item, i) in filterBy()"
        :key="`film_${i}`"
        :id="`film_${i}`"
      >
        <Card :item="item" :genres="genres" />
      </li>
    </ul>
    <div class="back-to-top">
      <button
        class="btn btn--back-to-top"
        @click="topFunction"
        :class="[scrolled ? showInline : hide]"
      >
        Back to top
      </button>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import Card from "./Card.vue";
export default {
  name: "Catalog",
  components: {
    Card,
  },
  data() {
    return {
      catalog: null,
      filterString: "",
      showInline: "show-inline",
      hide: "hide",
      scrolled: false,
      genres: [],
    };
  },
  props: {
    titleRequest: String,
  },
  created() {
    this.titleRequest = "Hello";
    axios
      .all([
        axios.get("https://api.themoviedb.org/3/genre/tv/list", {
          params: {
            api_key: "8eed27c438ed455893587d87d2a0d9d5",
          },
        }),
        axios.get("https://api.themoviedb.org/3/genre/movie/list", {
          params: {
            api_key: "8eed27c438ed455893587d87d2a0d9d5",
          },
        }),
      ])
      .then(
        axios.spread((tv, movie) => {
          this.genres = tv.data.genres.concat(movie.data.genres);
        })
      );
  },
  watch: {
    titleRequest: function (e) {
      const params = {
        api_key: "8eed27c438ed455893587d87d2a0d9d5",
        query: e,
      };
      axios
        .all([
          axios.get("https://api.themoviedb.org/3/search/movie", {
            params: params,
          }),
          axios.get("https://api.themoviedb.org/3/search/tv", {
            params: params,
          }),
        ])
        .then(
          axios.spread((res1, res2) => {
            //catalog
            res1.data.results.forEach((e) => {
              (e.category = "movie"), (e.nameTitle = e.title);
            });
            res2.data.results.forEach((e) => {
              (e.category = "tv"), (e.nameTitle = e.name);
            });
            const totalData = res1.data.results.concat(res2.data.results);
            this.catalog = totalData.sort((a, b) =>
              a.nameTitle.localeCompare(b.nameTitle)
            );
            //get cast//genres
            this.catalog.forEach((e) => {
              e.cast_list = [];
              e.genresList = [];
              axios
                .get(
                  `https://api.themoviedb.org/3/${e.category}/${e.id}/credits`,
                  {
                    params: {
                      api_key: "8eed27c438ed455893587d87d2a0d9d5",
                    },
                  }
                )
                .then((res) => {
                  let i = 0;
                  while (i < 5 && i < res.data.cast.length) {
                    e.cast_list.push(res.data.cast[i].name);
                    i++;
                  }
                  e.genresList = e.genre_ids.map((e) => {
                    for (let i = 0; i < this.genres.length; i++) {
                      if (this.genres[i].id == e) {
                        return this.genres[i].name;
                      }
                    }
                  });
                  e.genresList.join(", ");
                });
            });
          })
        );
      //!then
    },
  },
  methods: {
    filterBy() {
      return this.catalog.filter((e) =>
        e.category.toLowerCase().includes(this.filterString.toLowerCase())
      );
    },
    topFunction() {
      this.$refs = 0;
      document.documentElement.scrollTop = 0;
    },
    scroll() {
      window.onscroll = () => {
        window.pageYOffset > 1000
          ? (this.scrolled = true)
          : (this.scrolled = false);
      };
    },
  },
  mounted() {
    this.$parent.$on("filterMovie", (filter) => {
      this.filterString = filter;
    });
    this.scroll();
  },
};
</script>

<style lang="scss" scoped>
@import "../assets/style/common";

.title-request {
  padding: 2rem 0;
  font-size: 1.5rem;
  span {
    font-style: italic;
  }

  h2 {
    display: inline-block;
    font-size: 3rem;
    font-weight: 400;
    margin: 0 $gutter-md;
  }
}

.catalog {
  $this: &;
  @include flex--SB-C;
  flex-wrap: wrap;
  position: relative;

  .catalog__item {
    position: relative;
    width: calc(20% - ($gutter-xs * 2));
    height: 30rem;
    margin: 0 $gutter-xs;
    margin-bottom: $gutter-sm;
    offset-anchor: center;

    &::after {
      content: "";
      position: absolute;
      bottom: 0;
      width: 100%;
      height: 50%;
      z-index: 1;
      display: inline-block;
      box-shadow: inset 0px -90px 70px -50px #000;
      transition: box-shadow 1.2s;
    }

    &:hover {
      &::after {
        content: "";
        position: absolute;
        bottom: 0;
        width: 100%;
        height: 50%;
        z-index: 1;
        display: inline-block;
        box-shadow: none;
      }
    }

    &:last-child {
      margin-right: auto;
    }
  }
}

.related {
  @include flex--ST-ST;
  margin-bottom: $gutter;

  .related__title {
    flex-shrink: 0;
  }

  .list__item {
    display: inline-block;
    margin: 0 0.4rem;
  }
}

.back-to-top {
  position: fixed;
  right: $gutter-lg;
  bottom: $gutter-md;
  z-index: 10;
}
</style>