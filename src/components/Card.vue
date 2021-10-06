<template>
    <div class="card">
        <h3 class="card">{{item.nameTitle}}</h3>
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
        </ol>
    </div>
</template>

<script>
export default {
    name:"Card",
    props: {
        item: Object
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

}
</style>