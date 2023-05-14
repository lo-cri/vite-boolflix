<script>

import { store } from '../store';
import axios from 'axios';

const API_KEY = '4da7dcca1d0417941fc8e999936345fe'

export default {
    name: 'CardsDefault',

    data() {
        return {
            store
        }
    },

    created() {
        this.popularMovies()
    },

    methods: {

        popularMovies() {
            axios.get(`https://api.themoviedb.org/3/movie/popular?api_key=${API_KEY}&language=it-IT`).then((res) => {
                console.log(res)
                return this.store.staticMovieArray = res.data
            })
        },

        flagApi(nation) {

            let flag = `https://flagsapi.com/${nation}/flat/64.png`

            if (nation == 'ZH') {
                return `https://flagsapi.com/CN/flat/64.png`
            }else if (nation == 'EN'){
                return `https://flagsapi.com/GB/flat/64.png`
            
            } else if (nation == 'JA') {
                 return 'https://flagsapi.com/JP/flat/64.png'

            } else if (nation == 'HI') {
                 return 'https://flagcdn.com/h40/us-hi.png'

            } else if (nation == 'KO') {
                 return 'https://flagsapi.com/KR/flat/64.png'

            } else {
                return flag
            }

        },

        imageApi(image) {
            let path = `http://image.tmdb.org/t/p/w342${image}`
            return path
        },

        maxVote(vote) {

            if (vote + 1 >= 9.1) {

                return vote = 5

            } else {

                vote = Math.ceil(vote)
                return vote = Math.ceil(0.5*(1+vote))

            }
        }
    }
}

</script>

<template>
    <div class="d-flex overflow-auto align-content-start flex-wrap">
        <div v-for="(element, index) in store.staticMovieArray.results" :key="index">
            <div class="bg-container position-relative single-card">
                <img :src="imageApi(element.poster_path)" alt="">

                <div class="texts-up position-absolute top-0 p-4">
                    <p>
                        <strong>Titolo:</strong> {{ element.title }}
                    </p>
                    <p>
                        <strong>Titolo originale:</strong> {{ element.original_title }}
                    </p>
                    <img v-if="element.original_language" class="different-flag" :src="flagApi(element.original_language.toUpperCase())" alt="">
                    <p v-else-if="element.original_language == null">
                        <strong>Lingua:</strong> Not Found
                    </p>
                    <p v-else>
                        <strong>Lingua:</strong> Not Found
                    </p>
                    <p>
                        <strong>Voto:</strong> {{ maxVote(element.vote_average) }}
                        <span v-for="n in 5" :key="n">
                            <i v-if="n <= maxVote(element.vote_average)" class="yellow-star fa-solid fa-star"></i>
                            <i v-else class="fa-regular fa-star"></i>
                        </span>
                    </p>
                    <p v-if="element.overview">
                        <strong>Overview:</strong> {{ element.overview }}
                    </p>
                    <p v-else>
                        <strong>Overview: None</strong>
                    </p>
                </div>
            </div>
        </div>
    </div>
</template>

<style scoped>
.bg-container {
    background-color: rgb(20, 20, 20);
}
.flex-wrap{
    margin: 5px;
}

.different-flag {
    height: 1rem;
}

.yellow-star {
    color: yellow !important;
}

p {
    margin-bottom: 0;
}

.texts-up {
    display: none;
}

.single-card{
    margin: 5px;
}

.single-card:hover .texts-up {
    display: block;
    background-color: rgba(0, 0, 0, 0.9);
    width: 100%;
}
</style>