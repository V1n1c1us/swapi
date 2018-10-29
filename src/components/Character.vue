<template>
   <div class="col-md-4">
      <div class="card character">
         <div class="card__border"></div>
            <header class="card__header">
               <h4 class="card__title">Name: {{character.name}}</h4>
               <p class="card__subtitle">19BBY</p>
            </header>
            <p class="cardInfo"><span class="cardInfo__label">Height:</span> <span class="cardInfo__value">{{character.height}}</span></p>
            <p class="cardInfo"><span class="cardInfo__label">Mass:</span> <span class="cardInfo__value">{{character.mass}}</span></p>
            <p class="cardInfo"><span class="cardInfo__label">Skin Color:</span> <span class="cardInfo__value">{{character.skin_color}}</span></p>
            <p class="cardInfo"><span class="cardInfo__label">Species:</span> <span class="cardInfo__value">{{species.name}}</span></p>
            <p class="cardInfo"><span class="cardInfo__label">Homeworld:</span> <span class="cardInfo__value">{{homeworld}}</span></p>

         <b-button @click="showModal">
            Info
         </b-button>
      </div>
      <b-modal ref="myModalRef" hide-footer title="Starships">
         <div class="d-block text-center">
            <p class="cardInfo"><span class="cardInfo__label">Name:</span> <span class="cardInfo__value">{{starships.name}}</span></p>
            <p class="cardInfo"><span class="cardInfo__label">Model:</span> <span class="cardInfo__value">{{starships.model}}</span></p>
            <p class="cardInfo"><span class="cardInfo__label">Manufacturer:</span> <span class="cardInfo__value">{{starships.manufacturer}}</span></p>
            <p class="cardInfo"><span class="cardInfo__label">Passengers:</span> <span class="cardInfo__value">{{character.starships.passengers}}</span></p>
            <p class="cardInfo"><span class="cardInfo__label">Passengers:</span> <span class="cardInfo__value">{{starships.length}}</span></p>
            <div v-for="(veic,i) in vehicles" :key="i">
               <p class="cardInfo"><span class="cardInfo__label">Vehicles:</span> <span class="cardInfo__value">{{vehicles}}}</span></p>
            </div>
         </div>
         <b-btn class="mt-3" variant="outline-danger" block @click="hideModal">X</b-btn>
      </b-modal>
   </div>
   <!--</div>-->
</template>
<script>
	import axios from 'axios'
	export default {
		props: ['character'],
		data() {
			return {
				planet: {},
				homeworld: '',
				species: '',
				starships: { },
				vehicles: { },
                peopl: {}
			}
		},

		methods: {
			listarPessoas () {
				return axios.get("https://swapi.co/api/people/")
			},
			listarStarships () {
				return axios.get("https://swapi.co/api/starships/")
			},
			listarVehicles () {
				return axios.get("https://swapi.co/api/vehicles/")
			},
			async loadData() {
				var vm = this
				const [starships,vehicles] = await axios.all([this.listarStarships(), this.listarVehicles()], this.listarPessoas());

				vm.vehicles = vehicles.data.results
				vm.vehicles.forEach(function (value,index) {
				//	vm.vehicles[index]
				})
				vm.starships = starships.data.results
				vm.starships.forEach(function (value,index) {
					//console.log(vm.starships[index])
				})
                // console.log("VEIC" , vehicles.data.results)
				// console.log("NAV" , starships.data.results)
			},
			showModal () {
				this.$refs.myModalRef.show()
			},
			hideModal () {
				this.$refs.myModalRef.hide()
			}
        },
		created() {

			fetch(this.character.homeworld)
				.then(res => res.json())
				.then(json => this.planet = json);
			fetch(this.character.species)
				.then(res => res.json())
				.then(json => this.species = json);
		},
        mounted() {
			this.loadData()
        },
		watch: {
			planet: function() {
				this.homeworld = this.planet.name;
			},
		}
	}
</script>
<style>
   .card{
   height: 350px;
   margin: auto auto 20px;
   padding: 21px 16px;
   border-radius: 3px;
   background: -webkit-gradient(linear,left top,right bottom,from(#313639),color-stop(40%,#232220),to(#150d08));
   background: linear-gradient(top left,#313639,#232220 40%,#150d08);
   }
   .card__border {
      content: "";
      pointer-events: none;
      position: absolute;
      top: 2px;
      left: 2px;
      right: 2px;
      bottom: 2px;
      border-radius: 3px;
      border: 1px solid #eeaf69;
   }
   .card__header {
      margin-bottom: 24px;
   }
   .card__title {
      margin: 0;
      font-weight: 400;
      font-size: 26px;
      color: #fff;
   }
   .card__subtitle {
      color: #d1c0c0;
      margin: 0;
      font-size: 16px;
      line-height: 18px;
   }
   .cardInfo {
      display: block;
      margin: 0;
      line-height: 35px;
   }
   .cardInfo__label {
      display: inline;
      color: #d1c0c0;
      font-weight: 600;
      font-size: 16px;
      margin-right: 6px;
   }
   .cardInfo__value {
      display: inline;
      color: #bc9d06;
      font-size: 16px;
   }
</style>