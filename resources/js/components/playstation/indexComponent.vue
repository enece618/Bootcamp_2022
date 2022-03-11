<template>
    <div class="playstation">
        <div class="variables">
                <span v-if="this.decision == false">  es Negativo</span>
                <span v-else>  es positivo </span>
        </div>

        <div class="suma">
            
            <v-text-field
            v-model="numero1"
            label="Numero 1"
            type="number"
            hint="Ingrese el Primer Número"
          ></v-text-field>
          <v-text-field
            v-model="numero2"
            label="Numero 2"
            type="number"
            hint="Ingrese el Segundo Número"
          ></v-text-field>

          <h3>La suma es: {{suma}}</h3>

        </div>
        <div class="formulario">
            <h1>Formulario de Juegos de Playstation</h1>
            <v-form v-model="valid">
                <v-text-field
                    v-model="juego.nombre"
                    label="Nombre"
                    type="text"
                    hint="Ingrese el Nombre del Juego"
                ></v-text-field>
                <v-text-field
                    v-model="juego.descripcion"
                    label="Descripcion"
                    type="text"
                    hint="Ingrese la Descripción del Juego"
                ></v-text-field>
                <v-text-field
                    v-model="juego.precio"
                    label="Precio"
                    type="number"
                    hint="Ingrese el Precio del Juego"
                ></v-text-field>

                <v-btn
                @click="storeGame(juego)"
                >Guardar</v-btn>
                <pre>
                    {{juego}}
                </pre>
            </v-form>
        </div>
        <div class="lista" v-for="juego in juegos.data" :key="juego.id">
            <ul>
                <li>{{juego.name}} / {{juego.description}} / {{juego.price}} / <v-btn @click.stop="$set(dialogGame, juego.id, true)">Editar</v-btn> / <v-btn @click="deleteJuego(juego)">Eliminar</v-btn></li>
            </ul>

            <v-dialog v-model="dialogGame[juego.id]" scrollable max-width="500" max-height="600" :key="juego.id">
                    <v-card>
                        <v-card-title>
                            <span class="headline">Edit Game: {{juego.name}}</span> <v-spacer></v-spacer> <v-btn color="primary" icon text @click.stop="$set(dialogGame, juego.id, false)"><v-icon>mdi-close</v-icon></v-btn>
                        </v-card-title>
                        <v-card-text>
                        <v-container>
                            <v-layout row flex>
                                <v-flex sm12 md6>
                                    <v-text-field
                                        label="Name"
                                        v-model="juego.name"
                                    ></v-text-field>
                                </v-flex>
                                <v-flex sm12 md6>
                                    <v-text-field
                                        label="Description"
                                        v-model="juego.description"
                                    ></v-text-field>
                                </v-flex>
                                <v-flex sm12 md6>
                                    <v-text-field
                                        label="Price"
                                        v-model="juego.price"
                                        type="number"
                                    ></v-text-field>
                                </v-flex>
                            </v-layout>
                        </v-container>
                    </v-card-text>
                        <v-card-actions>
                            <v-spacer></v-spacer>
                            <v-btn text color="primary" @click="editJuego(juego)">actualizar</v-btn>
                        </v-card-actions>
                    </v-card>
            </v-dialog>
        </div>
    </div>
</template>

<script>
export default {
    data () {
        return {
            dialogGame: {},
            nombre: "Fernando",
            decision: false,
            numero1: 0,
            numero2: 0,
            juego: {
                nombre: '',
                descripcion: '',
                precio: 0,
                plataforma: 'Playstation'
            },
            valid: false,
            juegos: {}
            
            
        }
    },
    methods: {
        deleteJuego: function (game) {
            axios.delete('/games/' + game.id)
            
        },
        editJuego: function (game) {
            axios.put('/games/' + game.id, {
                name: game.name,
                description: game.description,
                price: game.price,
                platform: game.platform
            })
            .then((response) => {
                this.getGames();
            })
            

        },
        storeGame: function (game) {
            axios.post('/games', {
                name: game.nombre,
                description: game.descripcion,
                price: game.precio,
                platform: game.plataforma
            })
            .then((response) => {
                this.getGames();
            })
        },
        getGames() {
            axios.get('/get/games').then((response) => {
            this.juegos = response.data; 
        });
        },
    },
    computed: {
        suma() {
            
            let suma = 0

            suma = parseFloat(this.numero1) + parseFloat(this.numero2)

            return suma

        }

    },

    created () {
        this.getGames();
    }

}
</script>

<style>
 .playstation
  {
       background-color:aqua
  }   
</style>