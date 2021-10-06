<template>
    <div class="card">
        <h3 class="card">{{item.nameTitle}}</h3>
        <div class="card__poster">
            <img :src="`https://image.tmdb.org/t/p/w342${item.poster_path}`" :alt="item.nameTitle">
        </div>
        <ol class="card__list">
            <li class="card__item" v-if="item.category=='movie'">{{item.original_title}}</li>
            <li class="card__item" v-else>{{item.original_name}}</li>
            <li class="card__item flag-container">
                <img class="flag" 
                :src="`https://unpkg.com/language-icons/icons/${item.original_language}.svg`" 
                :alt="item.original_language" 
                v-if="item.original_language != ''&& item.original_language != 'cn' ">
                <img class="flag" :src="`https://unpkg.com/language-icons/icons/ch.svg`" :alt="item.original_language" v-else-if="item.original_language == 'cn'">
                <p v-else>Unknown Data</p>
            </li>
            <li class="card__item">{{item.vote_average}}</li>
            <li class="card__item">{{item.category}}</li>
            <li>{{stars(item.vote_average)}}</li>
            <li class="card__item">
                <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 250.12 238.03" 
                v-for="(item,i) in 5" 
                :key="`star${i}`" 
                :class="starCondition(i)">
                    <path class="cls-1" d="M314.4,545.94l22.52,96.7a1,1,0,0,0,1.06.77l98.92-8.47a1,1,0,0,1,.6,1.85l-85,51.3a1,1,0,0,0-.41,1.24l38.62,91.47a1,1,0,0,1-1.57,1.15l-75.06-65a1,1,0,0,0-1.31,0l-75,65a1,1,0,0,1-1.58-1.15l38.63-91.47a1,1,0,0,0-.41-1.24l-85-51.3a1,1,0,0,1,.61-1.85l98.92,8.47a1,1,0,0,0,1.06-.77l22.52-96.7A1,1,0,0,1,314.4,545.94Z" transform="translate(-188.37 -544.67)"/>
                </svg>
            </li>
        </ol>
    </div>
</template>

<script>
export default {
    name:"Card",
    props: {
        item: Object
    },
    data() {
        return {
            fullStar: "fullStar",
            emptyStar: "emptyStar"
        }
    },
    methods: {
        stars(e) {
            return parseInt(Math.ceil(e/2));
        },
        starCondition(val){
            val++;
            return val <= this.stars(this.item.vote_average) ? this.fullStar : this.emptyStar
        }
    }
}
</script>

<style lang="scss" scoped>

.card {
    $this: &;
    .flag-container {
            height: 1.2rem;
            width: 1.2rem;
            border-radius: 5px;
            overflow: hidden;
        .flag {
                width: 100%;
        }
    }

    svg{
        height: 1.5rem;

    }
    .fullStar {
        fill: #000;
    }

    .emptyStar {
        stroke: #000;
        stroke-width: .5rem;
        fill: transparent;
    }
    
}
</style>