<template>
    <div class="Main">
        <!-- NAV -->
        <div class="nav">
            <Slide right width="200">
                <a href="#wes">
                    <span>Wes</span>
                </a>
                <a href="#gb">
                    <span>The grand budapest hotel</span>
                </a>
                <a href="#mk">
                    <span>Moonrise kingdom</span>
                </a>
            </Slide>
        </div>
        <!-- HERO -->
        <div class="hero">
            <div class="hPic">
                <img src="./../assets/bgs/hero.png" alt="hero">
            </div>
            <div tabindex="-1" id="wes" class="title">
                <h2>Wes</h2>
                <h2>Anderson</h2>
                <!-- WES INFO -->
                <div class="wesInfo">
                    <div class="info">
                        <div class="pic">
                            <img src="./../assets/Wes-Anderson-01.jpg" alt="hero">
                        </div>
                        <div class="content">
                            <h3>Film director, screenwriter, and producer</h3>
                            <p class="fullname">Wesley Wales Anderson</p>
                            <div class="wesRow">
                                <p class="sText">Born:</p>
                                <p>{{ wesObject.birthday }}</p>
                            </div>
                            <div class="wesRow">
                                <p class="sText">Place of birth:</p>
                                <p>{{ wesObject.place_of_birth }}</p>
                            </div>
                            <div class="wesRow">
                                <p class="sText">Years active:</p>
                                <p>1992 – present</p>
                            </div>
                            <p class="sText bio">{{ wesObject.biography }}</p>
                        </div>
                        <div class="films">
                            <p><a href="#gb">The grand budapest hotel</a></p>
                            <p><a href="#mk">Moonrise kingdom</a></p>
                        </div>
                    </div>
                </div>
                <!-- GRAND BUDAPEST -->
                <div tabindex="-1" id="gb" class="film GB">
                    <Grandbudapest></Grandbudapest>
                </div>
                <!-- MOONRISE KINGDOM -->
                <!-- <div tabindex="-1" id="mk" class="film MK">
    <Moonrise></Moonrise>
</div> -->
            </div>
        </div>
    </div>
</template>
<script>
import * as HTTP from './../../http-common';
import { Slide } from 'vue-burger-menu'
var numeral = require("numeral");

import Grandbudapest from './films/Grandbudapest.vue';
import Moonrise from './films/Moonrise.vue';

export default {
    name: 'Showcase',
    components: {
        Slide,
        Grandbudapest,
        Moonrise
    },
    data() {
        return {
            wesObject: {},
            wesPic: '',
        }
    },
    beforeMount() {
        this.fetchWes()
    },
    methods: {
        // Fetch info about Wes
        fetchWes() {
            HTTP.HTTP.get(`person/5655?api_key=${process.env.VUE_APP_API_KEY}`)
                .then(response => {
                    this.wesObject = response.data
                    console.log(this.wesObject)
                })
                .catch(error => {
                    console.log(error)
                    this.showErr = true
                })
        },
        // Fetch Moonrise kingdom
        fetchMk() {
            HTTP.HTTP.get(`movie/120467?api_key=${process.env.VUE_APP_API_KEY}`)
                .then(response => {
                    this.mkObject = response.data
                    this.MKPoster = "https://image.tmdb.org/t/p/w500/" + `${this.mkObject.poster_path}`
                })
                .catch(error => {
                    console.log(error)
                    this.showErr = true;
                })
        },
    },
};
</script>
<style scoped lang="scss">
@import "./../assets/main.scss";

.Main {
    width: 100vw;
    height: 100vh;
    display: flex;
    flex-direction: column;
    position: relative;
    padding-top: 591.44px / 1127.34px * 100%;
}

// HERO
.hero {
    height: 20vh;
    width: 100vw;
    background-size: cover;
    background-position: center;

    .hPic img {
        height: auto;
        width: 100%;
        position: absolute;
        top: 0;
        left: 0;
        margin-top: -13%;
    }

    .title {
        height: 0;
        margin-top: -22%;
        position: absolute;

        h2 {
            font-family: $font-family-title;
            font-size: 70px;
            margin-left: 2%;
            float: left;
        }

        // WES INFO
        .wesInfo {
            display: flex;
            flex-direction: column;
            margin-top: 10rem;
            padding-bottom: 2vw;
            z-index: 1;
            height: auto;
            width: 100vw;
            background-color: $bg-white;
            margin-left: -3%;

            .info {
                margin-left: 5vw;
                margin-right: 3vw;
                display: flex;
                flex-direction: row;
                gap: 3rem;
                font-size: $font-size;

                .pic {
                    // width: 30vw;
                    flex-grow: 1;
                    flex-basis: 0;

                    img {
                        border-radius: 5px;
                        width: 100%;
                        height: auto;
                        box-shadow: 0 2.8px 2.2px rgba(0, 0, 0, 0.034), 0 6.7px 5.3px rgba(0, 0, 0, 0.048), 0 12.5px 10px rgba(0, 0, 0, 0.06), 0 22.3px 17.9px rgba(0, 0, 0, 0.072), 0 41.8px 33.4px rgba(0, 0, 0, 0.086), 0 100px 80px rgba(0, 0, 0, 0.12);
                    }
                }

                .films {
                    // width: 25%;
                    flex-grow: 1;
                    flex-basis: 0;
                    margin-top: 3rem;
                    margin-left: -1rem;
                    font-weight: 500;

                    p {
                        float: left;
                        margin-bottom: 1rem;
                    }
                }
            }
        }
    }
}

.content {
    // width: 40vw;
    flex-grow: 2;
    flex-basis: 0;

    .fullname {
        margin-top: 1%;
        margin-bottom: 5%;
    }

    .wesRow {
        display: flex;
        flex-direction: row;
        justify-content: left;
        margin-left: 20%;

        p {
            font-size: $font-size;
        }

        p:first-child {
            margin-right: 1rem;
        }
    }

    .bio {
        margin-top: 4%;
        margin-left: 12.5%;
        width: 75%;
    }
}


.filmRow {
    display: flex;
    flex-direction: row;

    p:first-child {
        margin-right: 0.5rem;
    }

    p:nth-child(2) {
        margin-right: 0.5rem;
    }
}

.sText {
    font-size: $font-size-stext;
    font-weight: 400;
}

.film {
    padding-top: 2vw;
    padding-left: 5vw;
    padding-bottom: 2vw;
    width: 100vw;
    overflow: hidden;
    z-index: 1;
    height: auto;
    font-size: $font-size;
}

// GRAND BUDAPEST
.GB {
    background-color: $bg-GB;
}

// MOONRISE KINGDOM
.MK {
    background-color: $bg-MK;
}


// NAVBAR
.nav {
    display: flex;
    z-index: 10;
}

.nav a {
    justify-content: center;
}

// MEDIA QUERIES
@media screen and (max-width: 900px) {
    .content {
        flex-grow: 1;

        .wesRow {
            flex-direction: column;

            p:first-child {
                margin: 0;
            }

            p:last-child {
                margin-bottom: 0.5rem;
            }
        }

    }
}

@media screen and (max-width: 769px) {
    .hero {
        .title h2 {
            font-size: 40px;
        }
    }
}

@media screen and (max-width: 500px) {
    .hero {
        .title h2 {
            font-size: 25px;
        }
    }
}
</style>