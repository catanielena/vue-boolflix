<template>
    <ul>
        <li v-for="(item,i) in catalog" :key="`film_${i}`">
            <h3>{{item.title}}</h3>
            <ul>
                <li>{{item.original_title}}</li>
                <li>{{item.otiginal_language}}</li>
                <li>{{item.vote_average}}</li>
            </ul>
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
    mounted() {
        this.$root.$on('titleInput', titleInput => {
            axios
                .get('https://api.themoviedb.org/3/search/movie', {
                    params: {
                        api_key: "8eed27c438ed455893587d87d2a0d9d5",
                        language: "it-IT",
                        query:titleInput
                    }
                })
                .then((res) => this.catalog = res.data.results)
        })
    }
}

</script>

<style lang="scss" scoped>

</style>