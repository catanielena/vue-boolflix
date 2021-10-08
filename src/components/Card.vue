<template>
    <div class="card">
        <!-- card__poster -->
        <div class="card__poster" :class="[active ? hide : show]" @click="mouseOver">
            <img :src="`https://image.tmdb.org/t/p/w342${item.poster_path}`" :alt="item.nameTitle" v-if="item.poster_path">
            <div class="missing--image" v-else>
                <h3 class="card">{{item.nameTitle}}</h3>
            </div>
        </div>
        <!-- card__list -->      
        <ol class="card__list" :class="[active ? show : hide]" @mouseleave="mouseLeave">
            <!-- card__film-title -->      
            <li><h3 class="card">{{item.nameTitle}}</h3></li>
            <!-- card__film-original-title -->      
            <li class="card__item f--size-md text--grey">{{item.original_title || item.original_name}}</li>
            <!-- card__film-language-flag -->      
            <li class="card__item flag-container item--m-bottom--auto">
                <img class="flag" 
                :src="`https://unpkg.com/language-icons/icons/${posterPath(item.original_language)}.svg`" 
                :alt="item.original_language" 
                v-if="posterPath(item.original_language)">
                <!-- <img class="flag" :src="`https://unpkg.com/language-icons/icons/ch.svg`" :alt="item.original_language" v-else-if="item.original_language == 'cn'"> -->
                <p v-else>Unknown Data</p>
            </li>
            <!-- card__genres -->      
            <li class="card__item item--genres">{{genresString()}}</li>
            <!-- cast__cast -->      
            <li class="card__item" v-if="item.cast_list != 0">
                <ul>
                    <li class="text--grey f--size-sm">Cast:</li>
                    <li class="f--size-sm" v-for="actor in item.cast_list" :key="`${item.nameTitle}_${actor}`">{{actor}}</li>
                </ul>
            </li>
            <!-- cast__MDB Rating-->      
            <li class="card__item text--right f--size-sm">MDB Rating {{item.vote_average}}</li>
            <!-- cast__MDB Rating-stars-->      
            <li class="card__item item-stars text--right">
                <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 143.95 137.32"
                v-for="(item,i) in 5" 
                :key="`star${i}`" 
                :class="starCondition(i)">>
                    <path class="cls-1" d="M193.21,252.74l19.23,39a3.69,3.69,0,0,0,2.78,2l43,6.25a3.69,3.69,0,0,1,2,6.29l-31.11,30.33a3.68,3.68,0,0,0-1.06,3.27l7.35,42.82a3.69,3.69,0,0,1-5.36,3.89l-38.46-20.22a3.71,3.71,0,0,0-3.43,0l-38.46,20.22a3.69,3.69,0,0,1-5.35-3.89l7.34-42.82a3.68,3.68,0,0,0-1.06-3.27l-31.11-30.33a3.69,3.69,0,0,1,2-6.29l43-6.25a3.69,3.69,0,0,0,2.78-2l19.23-39A3.69,3.69,0,0,1,193.21,252.74Z" transform="translate(-117.93 -250.19)"/>
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
            emptyStar: "emptyStar",
            hide: "hide",
            show: "show",
            active: false
        }
    },
    methods: {
        stars(e) {
            return parseInt(Math.ceil(e/2));
        },
        starCondition(val){
            val++;
            return val <= this.stars(this.item.vote_average) ? this.fullStar : this.emptyStar
        },
        mouseOver: function(){
            this.active = true;   
        },
        mouseLeave: function(){
            this.active = false;   
        },
        posterPath: function(lang) {
            if (lang != ''&& lang != 'cn' ) {
                return lang
            } else if (lang == 'cn') {
                return "ch"
            } else {
                return false
            }
        },
        genresString() {
            return this.item.genresList.join(', ');
        }
    }
}
</script>

<style lang="scss" scoped>
@import '../assets/style/common';

.card {
    width: 100%;
    height: 100%;
    $this: &;
    position: relative;
    overflow: hidden;
    cursor: pointer;
    &__poster {
        z-index: 1;
        height: 100%;
        img {
            @include objFit--C-T;
            display: block;
        }
    }

    &__list {
        position: absolute;
        display: flex;
        flex-direction: column;
        top: 0;
        height: 100%;
        width: 100%;
        padding: $gutter-md $gutter-md 5rem $gutter-md;
        background-color: #000;

        .card__item {
            width: 100%;
        }

    }
    .flag-container {
        height: 1.2rem;
        width: 1.2rem;
        // border-radius: .3125rem;
        // overflow: hidden;
        .flag {
                width: 100%;
        }
    }

    svg{
        width: 1.3rem;
        margin: $gutter-sm $gutter-xs;
        stroke-miterlimit: 10;
    }

    .fullStar {
        fill: $yellow;
    }

    .emptyStar {
        stroke: $yellow;
        stroke-width: .25rem;
        fill: transparent;
    }

    .hide {
        display: none;
        @include animation-leave;
    }

    .show {
        display: flex;
        @include animation-enter;
    }

    .item--m-bottom--auto {
        margin-bottom: auto;
        margin-top: $gutter-sm;
    }    


    .missing--image {
        width: 100%;
        height: 100%;
        padding: $gutter-md $gutter-md 5rem $gutter-md;
        background-image: url('../assets/img/bg_poster.jpg');
        background-position: center;
        background-size: cover;
        background-repeat: no-repeat;

        h3 {
            font-size: 3rem;
        }
    }

    .item--genres {
        margin: $gutter-md 0;
        font-size: 1.2rem;
        font-weight: 700;
        color: #7a838c;
    }
}

@media screen and (max-width: 768px) {
    .card {
        .missing--image {

            h3 {
                font-size: 1.3rem;
            }
        }
        .item--genres {
            font-size: .7rem;
        }

        svg{
            width: .6rem;
        }
    }
}
</style>