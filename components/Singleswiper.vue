<template>
    <nuxt-link :to="getPlayUrl(_obj)" class="single-movie-block">
        <div class="poster-container">
            <nuxt-img class="poster" :loading="_fetchMode" :src="_obj.imageslide" height="1000" :alt="_obj.full_name" />
            <div class="poster-overlay">
                <!-- <div class="rating" v-show="_obj.ratescore">{{ _obj.ratescore }}</div> -->
                <!-- <b-icon-play-circle class="poster-play" /> -->
                <div class="title">{{ _obj.full_name }}</div>
                <div class="title-cate" style="display: flex">
                    <div>ประเภท :</div>
                    <div style="padding-left: 5px" v-for="(value, index) in _obj.Category" :key="index">
                        {{ value.name_th }}
                    </div>
                </div>
                 <div class="title-buttom" style="display: flex">
                    <div class="buttom">ดูหนัง</div>
                </div>
            </div>
        </div>

        <!-- <div class="d-flex align-items-center">
            <div class="resolution">{{ _obj.quality }}</div>
            <div class="sound flex-grow-1" v-show="_obj.sound_main">เสียง : {{ _obj.sound_main }}</div>
            <div class="view"><b-icon-eye class="view-icon" /> {{ _obj.view }}</div>
        </div> -->
    </nuxt-link>
</template>

<script>
export default {
    props: {
        _obj: {
            type: Object,
            required: true,
        },
        _fetchMode: {
            type: String,
            required: false,
            default: "lazy",
        },
    },
};
</script>

<style lang="css" scoped>
.poster {
    height: 500px !important;
}
.title {
    font-size: 30px;
}
</style>
