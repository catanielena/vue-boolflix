<template>
    <header class="header">
        <!-- <div class="header__navbar">
        </div>  -->
        <div class="navbar__logo">
            <img src='https://fontmeme.com/permalink/211007/c072857f28ffed69c9266a55183f4c20.png' alt="">
        </div>
        <nav class="navbar__nav">
            <ul class="nav__list">
                <li class="list__item">
                    <button type="button" class="btn f--size-sm" @click="filterCatalogBy(''); seriesActive=false; movieActive=false">All</button>
                </li>
                <li class="list__item">
                    <button type="button" class="btn f--size-sm btn-filter" @click="filterCatalogBy('movie'); movieActive = !movieActive; seriesActive=false" :class="{'active' : movieActive}">Movie</button>
                </li>
                <li class="list__item">
                    <button type="button" class="btn f--size-sm btn-filter" @click="filterCatalogBy('tv');seriesActive = !seriesActive; movieActive=false" :class="{'active' : seriesActive}">Series</button>
                </li>
            </ul>
        </nav>
        <form action="#">
            <input type="text" placeholder="Search" v-model="titleInput" @keyup.enter="sendTitle">
            <button type="button" class="btn" @click.prevent="sendTitle">
                <svg xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" version="1.1" x="0px" y="0px" viewBox="0 0 100 125" enable-background="new 0 0 100 100" xml:space="preserve">
                <path d="M99.337,89.242L68.052,57.956c4.063-6.057,6.246-13.179,6.246-20.636c0-9.923-3.864-19.252-10.881-26.269  C56.4,4.035,47.072,0.171,37.149,0.171c-9.923,0-19.252,3.864-26.269,10.88C3.864,18.068,0,27.397,0,37.32  s3.864,19.251,10.881,26.268s16.345,10.881,26.269,10.881c7.541,0,14.737-2.234,20.838-6.385l31.255,31.253  c1.055,1.056,2.995,0.826,4.332-0.511l5.252-5.252C100.164,92.237,100.393,90.297,99.337,89.242z M37.149,64.405  c-14.936,0-27.086-12.15-27.086-27.085s12.151-27.086,27.086-27.086c14.935,0,27.085,12.151,27.085,27.086  S52.084,64.405,37.149,64.405z"/>
                </svg>
            </button>
        </form>
    </header>
</template>

<script>
export default {
    name: "Header",
    data() {
        return {
            titleInput: "",
            movieActive: false,
            seriesActive: false
        }
    },
    methods: {
        sendTitle() {
            this.$emit('titleInput', this.titleInput),
            this.titleInput = ""
        },
        filterCatalogBy(filter) {
            this.$parent.$emit('filterMovie', filter)
        }
    }
}

</script>

<style lang="scss" scoped>
@import '../assets/style/common';

.header {
    $this: &;
    @include flex--SB-C;
    position: fixed;
    padding: $gutter $gutter-md;
    top: 0;
    z-index: 10;
    width: 100%;
    background-color: $mainColor;

    .header__navbar {
        @include flex--ST-C;
        margin-right: auto
    }

    .nav__list {
        @include flex--ST-C;
        margin: 0 5rem;

        .list__item {
            margin: 0 $gutter-md;
        }
    }

    .navbar__logo {
        height: 1.5rem;

        img {
            height: 100%;
        }
    }

    form {
        @include flex--x-C;
        justify-content: center;
        width: 100%;
        .btn {
            display: flex;
            margin: 0 $gutter;
            svg {
                margin-left: auto;
                fill: #fff;
                width: 1.25rem;
                
            }
        }
    }


    .btn-filter {
        // border: 1px solid #fff;
        color: $grey;
        padding: $gutter-sm $gutter-md;
        border-radius: .2rem;
        background-color: #2a2a2a;
        transition: color .5s;

        &:hover,
        &.active {
            color: #fff;
            background-color: #333333;
        }
    }
}
</style>