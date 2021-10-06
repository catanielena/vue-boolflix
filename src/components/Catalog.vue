<template>
    <ul class="catalog">
        <li class="catalog__item" v-for="(item,i) in catalog" :key="`film_${i}`">
            <Card :item="item"/>
        </li>
    </ul>
</template>

<script>
import axios from 'axios';
import Card from './Card.vue';
export default {
    name: "Catalog",
    components: {
        Card
    },
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
            axios.all([
                axios.get("https://api.themoviedb.org/3/search/movie", {
                    params: {
                        api_key: "8eed27c438ed455893587d87d2a0d9d5",
                        query: e
                    }
                }),
                axios.get("https://api.themoviedb.org/3/search/tv", {
                    params: {
                        api_key: "8eed27c438ed455893587d87d2a0d9d5",
                        query: e
                    }
                }),
            ])
            .then(axios.spread((res1, res2) => {
            res1.data.results.forEach(e => {
                e.category = "movie",
                e.nameTitle = e.title
            });
            res2.data.results.forEach(e => {
                e.category = "series",
                e.nameTitle = e.name
            });
            const totalData = res1.data.results.concat(res2.data.results);
            this.catalog = totalData.sort((a, b) => a.nameTitle.localeCompare(b.nameTitle));
            }));
        }
    }
}

</script>

<style lang="scss" scoped>


</style>