<template>
    <div class="games">
        <div class="create-games">
            <div class="text-right">
                <v-dialog
                v-model="dialog"
                width="500"
                >
                <template v-slot:activator="{ on, attrs }">
                    <v-btn
                    color="primary"
                    dark
                    v-bind="attrs"
                    v-on="on"
                    >
                    <v-icon small left>mdi-account</v-icon>
                    Add Games
                    </v-btn>
                </template>


                <v-card>
                    <v-card-title class="text-h5 grey lighten-2">
                    Add Game
                    </v-card-title>

                    <v-card-text>
                        <v-container>
                            <v-layout row flex>
                                <v-flex sm12 md6>
                                    <v-text-field
                                        label="Name"
                                        v-model="game.name"
                                    ></v-text-field>
                                </v-flex>
                                <v-flex sm12 md6>
                                    <v-text-field
                                        label="Description"
                                        v-model="game.description"
                                    ></v-text-field>
                                </v-flex>
                                <v-flex sm12 md6>
                                    <v-text-field
                                        label="Price"
                                        v-model="game.price"
                                        type="number"
                                    ></v-text-field>
                                </v-flex>
                                <v-flex sm12 md6>
                                    <v-text-field
                                        label="Platform"
                                        v-model="game.platform"
                                    ></v-text-field>
                                </v-flex>
                            </v-layout>
                        </v-container>
                    </v-card-text>

                    <v-divider></v-divider>

                    <v-card-actions>
                    <v-spacer></v-spacer>
                    <v-btn
                        color="primary"
                        text
                        @click="dialog = false, storeGames()"
                    >
                        Guardar
                    </v-btn>
                    </v-card-actions>
                </v-card>
                </v-dialog>
            </div>
        </div>
        <div class="table-games">
        <v-data-table
            :headers="headers"
            :items="games.data"
            :items-per-page="5"
            class="elevation-1"
        >
        <template v-slot:item="{ item }">
            <tr>
            <td>{{item.id}}</td>
            <td>{{item.name}}</td>
            <td>{{item.description}}</td>
            <td>{{item.price}}</td>
            <td>{{item.platform}}</td>
            <td>{{item.created_at}}</td>
            
            <td>
                <v-icon
                small
                class="mr-2"
                @click.stop="$set(dialogGame, item.id, true)"
            >
                mdi-pencil
            </v-icon>
            <v-dialog v-model="dialogGame[item.id]" scrollable max-width="500" max-height="600" :key="item.id">
                    <v-card>
                        <v-card-title>
                            <span class="headline">Edit Game: {{item.name}}</span> <v-spacer></v-spacer> <v-btn color="primary" icon text @click.stop="$set(dialogGame, item.id, false)"><v-icon>mdi-close</v-icon></v-btn>
                        </v-card-title>
                        <v-card-text>
                        <v-container>
                            <v-layout row flex>
                                <v-flex sm12 md6>
                                    <v-text-field
                                        label="Name"
                                        v-model="item.name"
                                    ></v-text-field>
                                </v-flex>
                                <v-flex sm12 md6>
                                    <v-text-field
                                        label="Description"
                                        v-model="item.description"
                                    ></v-text-field>
                                </v-flex>
                                <v-flex sm12 md6>
                                    <v-text-field
                                        label="Price"
                                        v-model="item.price"
                                        type="number"
                                    ></v-text-field>
                                </v-flex>
                                <v-flex sm12 md6>
                                    <v-text-field
                                        label="Platform"
                                        v-model="item.platform"
                                    ></v-text-field>
                                </v-flex>
                            </v-layout>
                        </v-container>
                    </v-card-text>
                        <v-card-actions>
                            <v-spacer></v-spacer>
                            <v-btn text color="primary" @click="editGame(item)">actualizar</v-btn>
                        </v-card-actions>
                    </v-card>
            </v-dialog>
                
            <v-icon
                small
                @click="deleteGame(item)"
            >
                mdi-delete
            </v-icon>
            </td>
            </tr>
        </template>
        </v-data-table>
    </div>
    </div>
</template>

<script>
  export default {
    data () {
      return {
        games: {},
        dialogGame: {},
        success: "",  
        game: {},  
        dialog: false, 
        headers: [
          {
            text: 'ID',
            align: 'start',
            sortable: false,
            value: 'id',
          },
          { text: 'Name', value: 'name' },
          { text: 'Description', value: 'description' },
          { text: 'Price', value: 'price' },
          { text: 'Platform', value: 'platform' },
          { text: 'Date', value: 'created_at' },
          { text: 'Acciones' },
        ],
      }
    },
    methods: {
        getGames() {
            axios.get('/get/games').then((response) => {
            this.games = response.data; 
        });
        },
        storeGames () {
            axios.post('/games', this.game).then((response) => {
            this.success = response.data; 
        });
        },
        editGame (item) {
            axios.put('/games/'+ item.id, item).then((response) => {
            this.success = response.data; 
        });
        },
        deleteGame(item) {
            axios.delete('/games/'+ item.id).then((response) => {
            this.success = response.data; 
        });
        }
    },
    created () {
      this.getGames();  
    }
  }
</script>