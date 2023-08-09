<template>
    <div class="header">
        <h1>POKEMON GAME</h1>
    </div>
    <h2 v-if="!pokemon"> Espera por favor ...</h2>
    <div class="container" v-else="pokemon">
        <div>
            <Vidas :vidas="vidas" :vidasRestantes="vidasRestantes" />
        </div>
        <br>
        <h2>¿Quién es este Pokémon?</h2>
        <!--imagen-->
        <PokemonPicture :pokemonId="pokemon.id" :showPokemon="showPokemon" />
        <!--opciones-->
        <PokemonOptions :pokemons="pokemonArr" @selection-pokemon="checkAnswer" />

        <div v-show="message" class="message">
            <h2>
                {{ message }}
            </h2>

            <button v-if="vidasRestantes != 0" class="button" @click.prevent="newGame">
                Jugar de nuevo
            </button>
            <div v-else>
                <button class="button" @click.prevent="restarGame">
                    Reiniciar partida
                </button>

            </div>
        </div>

    </div>
    <div>
        <Score :puntaje="puntaje" />
        <div class="tablecontainer">
            <h2>Tabla de Puntajes</h2>
            <table>
                <thead>
                    <tr>
                        <th>Jugador</th>
                        <th>Puntos</th>
                    </tr>
                </thead>
                <tbody>
                    <tr v-for="(puntaje, index) in tablaPuntajes" :key="index">
                        <td>{{ puntaje.nombre }}</td>
                        <td>{{ puntaje.puntaje }}</td>
                    </tr>
                </tbody>
            </table>
        </div>

    </div>
    <br>
    Creado por Agu en el curso de Vue 3 [practicas de Js] 2023.
</template>

<script>
import PokemonPicture from '@/components/PokemonPicture.vue'
import PokemonOptions from '@/components/PokemonOptions.vue'
import getPokemonOptions from '@/helpers/getPokemonOptions'
import Vidas from '@/components/Vidas'
import Score from '@/components/Score'


export default {
    name: 'PokemonPage',
    components: { PokemonPicture, PokemonOptions, Vidas, Score },
    data() {
        return {
            pokemonArr: [],
            tablaPuntajes: [],
            pokemon: null,
            showPokemon: false,
            showAnswer: false,
            message: '',
            vidas: 3,
            vidasRestantes: 3,
            puntaje: 0,
        }
    },
    methods: {
        async mixPokemonsArray() {
            this.pokemonArr = await getPokemonOptions()
            const rndInt = Math.floor(Math.random() * 4)
            this.pokemon = this.pokemonArr[rndInt]
        },
        guardarPuntajeEnTabla() {
            const nombreJugador = prompt("Ingresa tu nombre:")
            if (nombreJugador) {
                this.tablaPuntajes.push({ nombre: nombreJugador, puntaje: this.puntaje })
                localStorage.setItem('tablaPuntajes', JSON.stringify(this.tablaPuntajes))
            }
        },
        checkAnswer(selectedId) {
            this.showPokemon = true
            this.showAnswer = true

            if (selectedId === this.pokemon.id) {
                this.message = `Correcto, ${this.pokemon.name}`
                this.puntaje += 10
            } else {

                this.message = `Te quedan  ${this.vidasRestantes - 1} vidas, fallaste la respuesta correcta es ${this.pokemon.name}`
                this.vidasRestantes -= 1
            }

            if (this.vidasRestantes === 0) {
                this.message = 'Juego terminado'
                this.guardarPuntajeEnTabla()
            }

        },
        newGame() {
            this.showPokemon = false,
                this.showAnswer = false,
                this.pokemonArr = [],
                this.pokemon = null,
                this.message = '',
                this.mixPokemonsArray()
        },
        restarGame() {
            this.showPokemon = false,
                this.showAnswer = false,
                this.pokemonArr = [],
                this.pokemon = null,
                this.message = '',
                this.vidasRestantes = this.vidas,
                this.puntaje = 0,
                this.mixPokemonsArray()
        }
    },
    mounted() {
        this.mixPokemonsArray()
        // Recuperar datos del localStorage
        const tablaPuntajes = localStorage.getItem('tablaPuntajes')
        if (tablaPuntajes) {
            this.tablaPuntajes = JSON.parse(tablaPuntajes)
        }
    }
}
</script>

<style scoped>
.header {
    display: flex;
    flex-direction: row;
    align-items: center;
    box-shadow: 3px 9px 5px -7px rgba(0, 0, 0, 0.29);
    background-color: #e74c3c;
    border-radius: 12px;
    justify-content: center;
    top: 0px;
    margin-left: 5%;
    margin-right: 5%;
    margin-bottom: 12px;
   
}

@media (max-width: 400px) {
    .header {
        display: flex;
        flex-direction: row;
        align-items: center;
        height: 75px;
        box-shadow: 3px 9px 5px -7px rgba(0, 0, 0, 0.29);
        background-color: #e74c3c;
        border-radius: 12px;
        justify-content: center;
        margin-bottom: 12px;
        top: 0px;
        margin-left: 2%;
        margin-right: 2%;
    }



}

h1 {
    font-family: 'Pokemon Solid', sans-serif;
    color: #ffffff;
    font-size: 2rem;
}

.input {
    margin-bottom: 24px;
    width: 80%;
    height: 28px;
    border-radius: 20px;
    border: 1px solid #ccc;
    padding: 8px;
    font-size: 16px;
    outline: none;
}

.container {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;

}

.message {
    border-radius: 25px;
    width: 300px;
    margin-bottom: 20px;
    position: absolute;
    margin-top: 420px;
    background: rgb(255, 255, 255);
    box-shadow: 0 8px 32px 0 rgba(31, 38, 135, 0.37);
    border: 1px solid rgba(255, 255, 255, 0.18);
}

.button {
    display: inline-block;
    padding: 10px 20px;
    font-size: 16px;
    border: none;
    border-radius: 5px;
    cursor: pointer;
    background-color: #e74c3c;
    color: white;
    margin-bottom: 20px;
}

.button:hover {
    background-color: #fa7f45;
}

.action-button {
    background-color: #ff3822;
}

.tablecontainer {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;

}

table {
    border-collapse: collapse;
    width: 40%;
    margin-top: 10px;
}

th,
td {
    border: 1px solid #dddddd;
    text-align: left;
    padding: 8px;
}

th {
    background-color: #f2f2f2;
}</style>
