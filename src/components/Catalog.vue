<template>
    <ul class="catalog">
        <li class="catalog__item" v-for="(item,i) in catalog" :key="`film_${i}`">
            <h3 class="item__title">{{item.title}}</h3>
            <ol class="item__list">
                <li class="item__item">{{item.original_title}}</li>
                <li class="item__item item--circle">
                    <img class="flag" :src="`https://unpkg.com/language-icons/icons/${item.original_language}.svg`" :alt="item.original_language" v-if="item.original_language != ''">
                    <p v-else>Unknown</p>
                </li>
                <li class="item__item">{{item.vote_average}}</li>
            </ol>
        </li>
    </ul>
</template>

<script>
import axios from 'axios';

export default {
    name: "Catalog",
    data() {
        return {
            catalog: null
        }
    },
    props: {
        titleRequest: String
    },
    watch: {
        titleRequest:  function(e) {
            axios
                .get('https://api.themoviedb.org/3/search/movie', {
                    params: {
                        api_key: "8eed27c438ed455893587d87d2a0d9d5",
                        language: "it-IT",
                        query: e
                    }
                })
                .then((res) => this.catalog = res.data.results)
        }
    }
}

</script>

<style lang="scss" scoped>

.catalog {
    $this: &;

    &__item {
        
        .flag {
            height: 1.2rem;
        }

        .item--circle {
            height: 1.2rem;
            width: 1.2rem;
            border-radius: 50%;
            overflow: hidden;
        }
    }
}
</style>