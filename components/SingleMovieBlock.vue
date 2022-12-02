<template>
    <nuxt-link :to="getPlayUrl(_obj)" class="single-movie-block-right">
        <div class="poster-container">
            <nuxt-img class="poster" :loading="_fetchMode" :src="_obj.picture" height="287" :alt="_obj.full_name" />
            <div class="title">{{ _obj.full_name }}</div>
            <div class="resolution" style="left: 15px;">{{ _obj.quality }}</div>
            <div class="rating" style="right: 10px;" v-show="_obj.ratescore">{{ score ? score : _obj.ratescore }}</div>
            <div class="poster-overlay">
                <div>
                    <div class="resolution">{{ _obj.quality }}</div>
                    <div class="rating" v-show="_obj.ratescore">{{ score ? score : _obj.ratescore }}</div>
                </div>

                <div style="margin-top: 25px">
                    <div class="">{{ _obj.full_name }}</div>
                    <div class="description">{{ _obj.description }}</div>
                    <div>
                        <div class="">ปีทีฉาย {{ _obj.year }}</div>
                        <div class="">เสียงภาค {{ _obj.sound_main }}</div>
                    </div>
                    <div style="display: flex">
                        <div>ประเภท :</div>
                        <div style="padding-left: 5px" v-for="(value, index) in _obj.Category" :key="index">
                            {{ value.name_th }}
                        </div>
                    </div>
                </div>

                <!-- <b-icon-play-circle class="poster-play" /> -->
            </div>
        </div>

        <!--
        <div class="d-flex align-items-center">
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
    data() {
        return {
            score: "",
        };
    },
    mounted() {
        this.getfixscore();
    },

    methods: {
        getfixscore() {
            this.score = Number.parseFloat(this._obj.ratescore).toFixed(1);
        },
    },
};
</script>

<style>
</style>
