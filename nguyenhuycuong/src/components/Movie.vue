<template>
   <div id="app">
    <h1>IMDB movies:</h1>
    <div class="nav">
        <button @click="ASC">Lowest rated</button>
        <button @click="DESC">Highest rated</button>
    </div>
    <ul>
        <li class="header">
            Movie name <span>Movie rating</span>
        </li>
        <li v-for="(movie, index) in titles" :key="index" v-bind:class="[{'blue' : movie.vote_average > 8.2}]" v-bind:style="{ opacity: movie.vote_average / 10 }"  >
            {{movie.title}} <span>{{movie.vote_average}}</span>
        </li>
    </ul>
</div>
</template>

<script>
const API_URL= "https://api.themoviedb.org/3/discover/movie?api_key=d993bdf37f8ab7c574c990434a85a69f&sort_by=popularity.desc";
export default {
   data() {
        return {
            columns: ['titles', 'rating'],
            titles: [
               
            ]
        }
    },
    methods: {
        ASC() {
            this.titles.sort((a, b) => a.vote_average > b.vote_average ? 1 : -1 )
        },
        DESC() {
            this.titles.sort((a, b) => a.vote_average < b.vote_average ? 1 : -1 )
        },
        shadeColor(color, percent) {
            var R = parseInt(color.substring(1,3),16);
            var G = parseInt(color.substring(3,5),16);
            var B = parseInt(color.substring(5,7),16);

            R = parseInt(R * (100 + percent) / 100);
            G = parseInt(G * (100 + percent) / 100);
            B = parseInt(B * (100 + percent) / 100);

            R = (R<255)?R:255;  
            G = (G<255)?G:255;  
            B = (B<255)?B:255;  

            var RR = ((R.toString(16).length==1)?"0"+R.toString(16):R.toString(16));
            var GG = ((G.toString(16).length==1)?"0"+G.toString(16):G.toString(16));
            var BB = ((B.toString(16).length==1)?"0"+B.toString(16):B.toString(16));

            return "#"+RR+GG+BB;
        }
    },
    mounted () {
        let self = this
        fetch(API_URL).then(function (response) {
            return response.json();
        }).then(function (result) {
            if(result)
            self.titles = result.results;
        })
    }
}

function shadeColor(color, percent) {

    var R = parseInt(color.substring(1,3),16);
    var G = parseInt(color.substring(3,5),16);
    var B = parseInt(color.substring(5,7),16);

    R = parseInt(R * (100 + percent) / 100);
    G = parseInt(G * (100 + percent) / 100);
    B = parseInt(B * (100 + percent) / 100);

    R = (R<255)?R:255;  
    G = (G<255)?G:255;  
    B = (B<255)?B:255;  

    var RR = ((R.toString(16).length==1)?"0"+R.toString(16):R.toString(16));
    var GG = ((G.toString(16).length==1)?"0"+G.toString(16):G.toString(16));
    var BB = ((B.toString(16).length==1)?"0"+B.toString(16):B.toString(16));

    return "#"+RR+GG+BB;
}
</script>
