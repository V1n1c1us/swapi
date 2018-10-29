<template>
  <div id="app">
    <div class="container">
      <img src="./assets/logo.png" alt="Icon" class="br-3">
      <h1 class="title">{{msg}}</h1>
      <div class="characters">
        <div class="row">
            <character v-for="(character,i) in characters" :key="i" :character="character"/>
        </div>

      </div>
      <button v-if="previousPage" class="btn" @click="fetchCharacters(previousPage)">
        <span aria-hidden="true">&laquo;</span>
        <span class="sr-only">Previous</span>
      </button>
      <button v-if="nextPage" class="btn" @click="fetchCharacters(nextPage)">
        <span aria-hidden="true">&raquo;</span>
        <span class="sr-only">Next</span>
      </button>
    </div>
  </div>
</template>



<script>
	import Character from './components/Character'
	export default {
		components: {

        },
		name: 'app',
		data () {
			return {
				msg: 'Star Wars Characters',
				result: {},
				vehicles: [],
				starships: [],
                peoples: []

			}
		},
		methods: {
			goNext(uri) {
				this.fetchCharacters(uri)
			},
			goPrevious(uri) {
			},
			fetchCharacters(uri = 'https://swapi.co/api/people/') {
				fetch(uri)
					.then(res => res.json())
					.then(json => this.result = json)
			},
		},
		created() {
			this.fetchCharacters();
		},
        mounted() {

            //
            // Pessoas.listarPeople().then( resposta => {
				// vm.peoples = resposta.data.results
            // //	console.log('PESSOAS', vm.peoples)
            // })
            // Vehicles.listarVehicles().then( resposta => {
				// vm.vehicles = resposta.data.results
            // //	console.log('Vehicles', vm.vehicles)
            // })
        },
		computed: {
			characters() {
				return this.result.results;
			},
			nextPage() {
				return this.result.next;
			},
			previousPage() {
				return this.result.previous;
			}
		},
		components: {
			Character
		}
	}
</script>

<style>
  #app {
    font-family: 'Avenir', Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #fff;
    margin-top: 60px;
  }
  body{
    background: #000000;  /* fallback for old browsers */
    background: -webkit-linear-gradient(to right, #434343, #000000);  /* Chrome 10-25, Safari 5.1-6 */
    background: linear-gradient(to right, #434343, #000000); /* W3C, IE 10+/ Edge, Firefox 16+, Chrome 26+, Opera 12+, Safari 7+ */

  }
  .title{
    font-family: 'Star Jedi', arial;
    color: #ffe61b;
    margin: 20px 0 40px 0;
    font-size: 50pt;
    text-shadow: 8px 8px 8px #000;
  }
  .btn {
    background: #000;
  }
  span {
    color: #fff;
  }
</style>
