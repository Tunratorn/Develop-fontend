<template>
    <div class="container-flu">
        <div class="container-popup" v-if="showmodal">
            <div class="flexbox">
                <div class="contacimg">
                    <div class="headderIMG">
                        <div class="titalheader">
                            <h1>Unsplash API</h1>
                        </div>
                        <div class="closeimg">
                            <span @click="showmodal = false">&times;</span>
                        </div>
                    </div>
                    <div class="flexboximg">
                        <img id="showimg" :src="urlimg">
                    </div>
                </div>
            </div>
        </div>
        <div class="contai-haed">
            <h1>Unsplash API</h1>
        </div>
        <div class="container-input">
            <input @keyup.enter="search" type="text" placeholder="Search">
        </div>
        <div class="contairner-mas" v-if="count === 0 && dataSearch === ''">
            <p>No images</p>
        </div>
        <div class="contairner-mas" v-else-if="count === 0 && dataSearch !== ''">
            <p>No images found from this keyword [{{ dataSearch }}]</p>
        </div>
        <div v-else>
            <div class="container-grid">
                <div class="contan-img" v-for="(img, index) in imgs.slice(0, show)">
                    <img :src="img.urls.raw" @click="zoom(img.urls.raw)">
                </div>
            </div>
            <div class="container-load" v-if="show < count">
                <div class="contan-btnload">
                    <button @click="loadmore(8)">Load more</button>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import axios from 'axios'

export default {
    data() {
        return {
            dataSearch: "",
            show: 8,
            urlapi: "https://api.unsplash.com/search/photos?client_id=AIPuVaxu1wewGBdNceoXbnHNe_A3BfHrPhOwt7A6DKY&query=",
            imgs: [],
            urlimg: "",
            showmodal: false,
            count: 0
        }
    },
    methods: {
        async search(event) {
            this.show = 8
            this.dataSearch = event.target.value
            let api = this.urlapi + this.dataSearch
            let result = await axios.get(api);
            this.imgs = result.data.results
            this.count = this.imgs.length
        },
        loadmore(n) {
            this.show += n
        },
        zoom(index) {
            this.urlimg = index
            this.showmodal = true
        },
    },

};

</script>

<style>
html,
body {
    margin: 0;
    padding: 0;
}

.titalheader {
    width: 100%;   
}

h1 {
    text-transform: uppercase;
}

.contairner-mas {
    text-align: center;
    margin-bottom: 200px;
    padding: 30px;
}

.headderIMG {
    display: flex;
    justify-content: center;
    align-items: center;
    padding-left: 30px;
    padding-right: 30px;
}

.container-flu {
    width: 100%;
    height: auto;
}

.closeimg {
    width: 100%;
    position: relative;
    text-align: end;
}

.flexbox {
    display: flex;
    justify-content: center;
    align-items: center;
    padding: 20px;
    margin: 10px;
}

.container-popup {
    background-color: rgba(0, 0, 0, 0.456);
    width: 100%;
    height: 100vh;
    position: fixed;
    overflow: auto;
}

.contacimg {
    height: auto;
    width: 100%;
    padding: 10px;
    background: #fff;
    display: grid;
    grid-template-rows: max-content;
}

span {
    font-size: 3rem;
    cursor: pointer;
}

.flexboximg {
    background-color: #fff;
    padding: 20px;
    display: flex;
    justify-content: center;
    align-items: center;
}

#showimg {
    height: auto;
    width: 1200px;
    max-width: 100%;
    cursor: auto;
}

.contai-haed {
    text-align: center;
    padding: 10px;
}

.container-input {
    padding: 10px;
    text-align: center;
}

input[type=text] {
    padding: 10px;
    width: 40%;
    border: 1px solid #ccc;
    border-radius: 5px;
    outline: none;
    font-size: 1rem;
}

button {
    padding: 10px;
    width: 150px;
    border: 1px solid #ccc;
    border-radius: 5px;
}

.container-grid {
    margin: 30px;
    display: grid;
    grid-template-columns: repeat(4, minmax(0, 1fr));
    grid-gap: 10px;
}

.container-load {
    text-align: center;
    padding: 10px;
}

img {
    width: 100%;
    height: 300px;
    cursor: zoom-in;
    object-fit: cover;
}

@media (max-width: 1400px) {
    .container-grid {
        grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
    }
}
</style>