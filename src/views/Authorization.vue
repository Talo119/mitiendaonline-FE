<template>
    <v-container fill-height>
        <v-row class="fila-registro"  justify="center" align="center">
            <v-col  cols="12" sm="6" md="6" lg="4">
                <v-card elevation="6">
                    <v-tabs
                        v-model="tab"
                        fixed-tabs
                        dark
                        icons-and-text
                        background-color="deep-purple accent-4"
                    >
                        <v-tabs-slider></v-tabs-slider>

                        <v-tab href="#tab-1">
                            Login
                            <v-icon>
                                fas fa-sign-in-alt
                            </v-icon>                            
                        </v-tab>
                        <v-tab href="#tab-2">
                            Registro
                            <v-icon>
                                fas fa-user-plus
                            </v-icon>                            
                        </v-tab>

                    </v-tabs>

                    <v-tabs-items v-model="tab">
                        <v-tab-item value="tab-1">
                            <v-card flat>
                                <v-card-text>
                                    <Login/>
                                </v-card-text>
                            </v-card>
                        </v-tab-item>
                        <v-tab-item value="tab-2">
                            <v-card flat>
                                <v-card-text>
                                    <Register/>
                                </v-card-text>
                            </v-card>
                        </v-tab-item>
                    </v-tabs-items>
                </v-card>
            </v-col>
        </v-row>
        
    </v-container>
  
</template>

<script>
import Register from '../components/Register'
import Login from '../components/Login'
export default {
    name:'Authorization',
    components: {
        Register, Login
    },
    data() {
        return {
            tab: null
        }
    },
}
</script>

<style>
   
</style>