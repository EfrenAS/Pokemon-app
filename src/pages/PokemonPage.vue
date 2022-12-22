<template>
  <h1 v-if="!pokemon">Espere un momento por favor...</h1>
  <div v-else>
    <h1>Quien es este Pokemon?</h1>
    <PokemonPicture
      :pokemonId="pokemon.id"
      :showPokemon="showPokemon"
    />
    <PokemonOptions
      :pokemons = "pokemonArr"
      @selection="checkAnswer"
    />
    <!--Optional maybe use TEMPLATE or some DIV to grouping this elements-->
    <div v-if="showAnswer">
      <h2>{{ message }}</h2>
      <button @click="newGame">
        Nuevo Juego
      </button>
    </div>
  </div>
</template>

<script>
  import PokemonPicture from '@/components/PokemonPicture'
  import PokemonOptions from '@/components/PokemonOptions'

  import getPokemonOptions from '@/helpers/getPokemonOptions'

  export default {
    name: 'PokemonPage',
    components: {
      PokemonPicture,
      PokemonOptions
    },
    data() {
      return {
        pokemonArr : [],
        pokemon: null,
        showPokemon: false,
        showAnswer: false,
        message: ''
      }
    },
    methods: {
      async mixPokemonArray() {
        this.pokemonArr = await getPokemonOptions()
        const rndInt = Math.floor( Math.random() * 4 )
        this.pokemon = this.pokemonArr[ rndInt ]
      },
      checkAnswer( selectedId ) {
        this.showPokemon = true
        this.showAnswer = true

        if(selectedId === this.pokemon.id) {
          this.message = `Lo capturaste!!! Es ${ this.pokemon.name }`
        } else {
          this.message = `Escapo!!! Era ${ this.pokemon.name }`
        }
      },
      newGame(){
        this.showPokemon = false
        this.pokemonArr = []
        this.pokemon = null
        this.mixPokemonArray()
        this.showAnswer = false
      }
    },
    mounted() {
      this.mixPokemonArray()
    }
  }
</script>