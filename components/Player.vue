<template>
    <div class="player-page">
        <div class="">
            <h2>{{ _isAV ? "ตัวอย่างหนัง:" : "" }} {{ _movieObj.full_name }}</h2>
        </div>
        <b-aspect aspect="16:5" class="mb-2" v-if="showmovie == true">
            <b-container>
                <b-row style="align-items: center">
                    <b-col cols="12" lg="6" xl="6" class="dis-play bg-color nopaddding">
                        <b-col cols="6" lg="6" xl="6" class="dis-play nopaddding">
                            <div class="preview-container-po">
                                <div class="preview-poster-po">
                                    <nuxt-img style="width: 100%" loading="lazy" format="webp" :src="_movieObj.picture" :alt="_movieObj.full_name" />
                                </div>
                            </div>
                            <!-- <div class="btn-play-mo" @click="showMovie()"><nuxt-img src="/Polygon.png" /> เล่น</div> -->
                        </b-col>
                        <b-col cols="6" lg="6" xl="6" class="dis-colum-center">
                            <div style="position: relative; top: -30px">
                                <div class="resolution">{{ _movieObj.quality }}</div>
                                <div class="rating" v-show="_movieObj.ratescore">{{ _movieObj.ratescore }}</div>
                            </div>
                            <div class="" style="margin-top: 25px">
                                <h6>{{ _movieObj.full_name }}</h6>
                            </div>

                            <div v-if="_movieObj.description">
                                <div class="movie-description">
                                    {{ _movieObj.description }}
                                </div>
                            </div>

                            <div class="">
                                {{ _movieObj.sound_main ? "เสียง : " + _movieObj.sound_main : "ไม่มี" }}
                            </div>
                            <div class="">
                                {{ _movieObj.year ? "ปีที่ฉาย : " + _movieObj.year : "ไม่มี" }}
                            </div>
                            <div style="display: flex">
                                <div>ประเภท :</div>
                                <div style="padding-left: 5px" v-for="(value, index) in _movieObj.Category" :key="index">
                                    {{ value.name_th }}
                                </div>
                            </div>

                            <!-- <div class="btn-play" @click="showMovie()"><nuxt-img src="/Polygon.png" /> เล่น</div> -->
                        </b-col>
                    </b-col>
                    <b-col cols="12" lg="6" xl="6">
                        <div class="preview-url" style="margin-top: 20px">
                            <iframe style="width: 100%; height: 350px; border-radius: 20px" class="preview-movie" :src="previewUrl" frameborder="0" allowfullscreen></iframe>
                        </div>
                    </b-col>
                </b-row>
            </b-container>
        </b-aspect>
        <b-aspect aspect="16:9" class="">
            <!-- <div class="iframe-container">
                <iframe id="playervideo" :src="playerIframeUrl" allowfullscreen="allowfullscreen" style="width: 100%; height: 100%; border: 0px; overflow: hidden"></iframe>
                <AdsVideo :_adsArray="adsVideoList" v-model="isShowAds" v-if="adsVideoList.length > 0 && isShowAds" />
            </div> -->
            <div class="iframe-container" style="z-index: 0; cursor: pointer" @click="Movie()">
                <nuxt-img format="webp" style="width: 100%" src="/playtem.png" alt="loader" />
            </div>
        </b-aspect>
        <div class="d-flex align-items-center justify-content-between bg-button">
            <div class="sound-container">
                <div class="btn-bot" v-if="_movieObj.player_link_main != '' && currentSound != 'Main'" @click="currentSound = 'Main'">ลิ้งค์หลัก ({{ _movieObj.sound_main }})</div>
                <div class="btn-bot" v-if="_movieObj.player_link_backup1 != '' && currentSound != 'Backup1'" @click="currentSound = 'Backup1'">ลิ้งค์สำรอง 1 ({{ _movieObj.sound_backup1 }})</div>
                <div class="btn-bot" v-if="_movieObj.player_link_backup2 != '' && currentSound != 'Backup2'" @click="currentSound = 'Backup2'">ลิ้งค์สำรอง 2 ({{ _movieObj.sound_backup2 }})</div>
            </div>
            <div class="btn-bot" @click="showReportPopup = true">แจ้งหนังเสีย</div>
        </div>

        <div class="custom-card-container mt-3" v-if="_type == 'series'">
            <div class="custom-card-header">เลือกตอน</div>
            <div class="custom-card-content">
                <div class="ep-list">
                    <nuxt-link :to="getEpLink(index + 1)" class="ep-row" :class="{ active: index + 1 == _ep }" v-for="(value, index) in epArray" :key="index" v-show="index + 1 != epArray.length">
                        EP {{ index + 1 }}
                    </nuxt-link>
                </div>
            </div>
        </div>
        <div class="popup-container" id="request-popup" v-show="showReportPopup" @click.self="showReportPopup = false">
            <div class="popup-content">
                <div class="text-right"><b-icon-x-circle-fill class="popup-close-btn" @click="showReportPopup = false" /></div>
                <div class="tab-btn-list">
                    <div class="tab-btn active">แจ้งหนังเสีย</div>
                </div>
                <div class="tab-content-list">
                    <div class="tab-content">
                        <textarea id="request-detail" style="height: 150px" class="popup-input" v-model="reportDetail" @keyup="isLetter"></textarea>
                        <div class="text-center">
                            <div class="submit-btn" @click="reportMovie()">ส่ง</div>
                        </div>
                    </div>
                </div>
            </div>
        </div>

        <!-- <div class="player-top-content">{{ _movieObj.year ? "ปีที่ฉาย : " + _movieObj.year : "" }} คะแนน : {{ _movieObj.ratescore }} / {{ _isAV ? 100 : 10 }}</div>
        <div class="movie-description">
            {{ _movieObj.description }}
        </div> -->

        <div class="" v-if="movieList.length">
            <div class="dis-between">
                <h2 class="list-title">หนังแนะนำ</h2>
            </div>
            <div class="" v-if="movieList">
                <swipercate />
            </div>
            <div class="dis-between">
                <h2 class="list-title">หนังAVแนะนำ</h2>
            </div>
            <div class="" v-if="movieList">
                <swiperAV />
            </div>
        </div>

        <div class="loader-container fixed" v-show="showLoader">
            <nuxt-img format="webp" src="/loader.png" alt="loader" />
        </div>
    </div>
</template>

<script>
export default {
    props: {
        _id: {
            type: Number,
            required: true,
            default: 0,
        },
        _slug: {
            type: String,
            required: true,
            default: "",
        },
        _ep: {
            type: String,
            required: false,
            default: "",
        },
        _type: {
            type: String,
            required: false,
            default: "movie",
        },
        _movieObj: {
            type: Object,
            required: true,
        },
        _isAV: {
            type: Boolean,
            required: false,
            default: false,
        },
    },
    data() {
        return {
            currentSound: "Main",
            playerIframeUrl: "",
            serieLinkMain: "",
            epArray: [],
            relateList: [],
            showReportPopup: false,
            reportDetail: "",
            adsVideoList: [],
            isShowAds: true,
            showLoader: false,
            showmovie: true,
            movieList: [],
            currentPage: 1,
            maxPage: 1,
        };
    },
    computed: {
        previewUrl: function () {
            if (this._movieObj.preview) return "https://www.youtube.com/embed/" + this._movieObj.preview + "?controls=0&rel=0";
            return "";
        },
        catIdArray() {
            let cat = [];
            for (const key in this._movieObj.Category) {
                if (Object.hasOwnProperty.call(this._movieObj.Category, key)) {
                    const element = this._movieObj.Category[key];
                    cat.push(element.id);
                }
            }
            return cat;
        },
    },
    created() {
        this.setPlayerUrl();
        this.getAdsVideo();
        this.getRelateMovies();

        if (this._isHot) {
            this.getHotMovies();
        } else if (this._isSearch) {
            this.getSearchMovie();
        } else {
            this.getMovies();
        }
    },
    mounted() {},
    watch: {
        currentSound() {
            const self = this;
            this.playerIframeUrl = "";
            setTimeout(() => {
                self.setPlayerUrl();
            }, 100);
        },
    },
    methods: {
        showMovie() {
            if (this.showmovie) {
                this.showmovie = false;
            }
        },
        setPlayerUrl() {
            if (this._type == "series") {
                if (this.currentSound == "Main") this.epArray = this._movieObj.player_series_link_main.split("!!end!!");
                if (this.currentSound == "Backup1") this.epArray = this._movieObj.player_series_link_backup1.split("!!end!!");
                for (let index = 0; index < this.epArray.length; index++) {
                    const element = this.epArray[index];
                    if (index + 1 == this._ep) this.playerIframeUrl = "https://movie.aegistrex.com/api/player/" + element.split("}}")[1];
                }
            } else {
                if (this._isAV) {
                    this.playerIframeUrl = "https://movie.aegistrex.com/av/api/player/" + this._movieObj.player_link_main;
                } else {
                    if (this.currentSound == "Main") this.playerIframeUrl = "https://movie.aegistrex.com/api/player/" + this._movieObj.player_link_main;
                    if (this.currentSound == "Backup1") this.playerIframeUrl = "https://movie.aegistrex.com/api/player/" + this._movieObj.player_link_backup1;
                    if (this.currentSound == "Backup2") this.playerIframeUrl = "https://movie.aegistrex.com/api/player/" + this._movieObj.player_link_backup2;
                }
            }
        },
        getRelateMovies() {
            const self = this;
            this.$axios
                .$post("movie/listmoviesimilar", {
                    category: self.catIdArray,
                    perpage: 4,
                    page: 1,
                })
                .then(function (response) {
                    if (response.code == 200) {
                        self.relateList = response.result;
                    }
                });
        },
        getEpLink(ep) {
            return "/" + this._type + "/" + this._id + "/" + this._slug + "/" + ep;
        },
        Movie() {
            this.$swal("", "กรุณาซื้อแพ็กเกจ", "warning");
        },
        reportMovie() {
            const detail = this.reportDetail;
            if (detail.trim() == "") {
                this.$swal("", "กรุณากรอกข้อมูลให้ครบ", "error");
                return;
            }
            this.$swal("", "กรุณาซื้อแพ็กเกจ", "warning");
            // const self = this;
            // this.showLoader = true;
            // let type_id = 0;
            // if (this._isAV) type_id = 1;
            // this.$axios
            //     .$post("moviebroken", {
            //         movie_id: self._id,
            //         movie_type: type_id,
            //         reason: detail,
            //     })
            //     .then(function (response) {
            //         self.showReportPopup = false;
            //         self.showLoader = false;
            //         self.reportDetail = "";
            //         if (response.code == 200) {
            //             self.$swal("", "สำเร็จ", "success");
            //         }
            //     });
        },
        getHotMovies() {
            const self = this;
            this.loadingList = true;
            this.$axios
                .$post("movie/listmovie", {
                    order: "ID",
                    orderby: "DESC",
                    types: "",
                    category: "",
                    year: 0,
                    perpage: 4,
                    page: this.currentPage,
                    search: this._search,
                })
                .then(function (response) {
                    if (response.code == 200) {
                        // console.log(response.result);
                        self.movieList = response.result;
                        self.maxPage = response.page_total;
                    }
                    self.loadingList = false;
                });
        },
        getMovies() {
            const self = this;
            this.loadingList = true;
            this.$axios
                .$post("movie/listmovie", {
                    order: "ID",
                    orderby: "DESC",
                    types: "",
                    category: "",
                    year: 0,
                    perpage: 6,
                    page: this.currentPage,
                    search: this._search,
                })
                .then(function (response) {
                    if (response.code == 200) {
                        // console.log(response.result);
                        self.movieList = response.result;
                        self.maxPage = response.page_total;
                    }
                    self.loadingList = false;
                });
        },
        getAdsVideo() {
            const self = this;
            this.$axios.$get("list/ads-video").then(function (response) {
                if (response.code == 200) {
                    self.adsVideoList = response.result ? response.result : [];
                }
            });
        },
    },
};
</script>
