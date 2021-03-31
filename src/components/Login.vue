<template>
   <v-form
        ref="form"
        v-model="valid"
        lazy-validation
        class=""
    >
        <v-progress-linear
            :active="cargando"
            :indeterminate="cargando"
            absolute
            top
            color="cyan"
        ></v-progress-linear>
        
        <v-text-field
            outlined
            v-model="login.email"
            :rules="emailRules"
            label="Email"
            required
            prepend-inner-icon="fas fa-at"   
            class="mt-5 pt-5"           
        >
        </v-text-field>

        <v-text-field
            outlined
            v-model="login.password"   
            :rules="passwordRules"         
            label="Password"
            required
            prepend-inner-icon="fas fa-lock"
            type="password"
            class="mb-5 pb-5"
        >            
        </v-text-field>
        

        <v-btn
            :disabled="!valid"            
            color="primary"            
            class="mr-4 white--text"
            @click="postLogin()"
        >
            Login
        </v-btn>

        <v-btn            
            color="grey"
            dark
            
        >
            Cancelar
        </v-btn>

        <v-snackbar
            v-model="notification.snackbar"
            :timeout="notification.timeout"            
            top
            :color="notification.color"            
            right
            elevation="6"
        >
            {{ notification.text }}

            <template v-slot:action="{ attrs }">
                <v-btn
                color="white"
                text
                v-bind="attrs"
                @click="notification.snackbar = false"
                >
                Close
                </v-btn>
            </template>
        </v-snackbar>

    </v-form>
</template>

<script>
import {mapState,mapActions} from 'vuex'
export default {
    data() {
        return {
            valid:true,
            loading:false,
            cargando: false,
            login: {
                email:'', password:''
            },
            notification:{
                snackbar: false,
                text: '',
                timeout: 3000,
                color:''
            },
            emailRules: [
                v => !!v || 'Email is required',
                v => /.+@.+\..+/.test(v) || 'Email must be valid',
            ],
            passwordRules: [
                v => !!v || 'Password is required',
                v => (v && v.length > 3) || 'Passowwrd must be more than 3 characters',
            ],
        }
    },

    computed: {
        ...mapState(['usuarioDB'])
    },

    methods: {
        ...mapActions(['guardarUsuario']),
        postLogin() {
            let self = this;
            self.cargando = true;
            this.axios.post('/users/login',this.login)
                .then(res =>{
                    console.log(res)
                    if (res.data === '2.Usuario y/o contraseña incorrecta.') {
                        this.showNotification('Usuario y/o contraseña incorrecta.','error')
                        this.cargando = false;
                        return 
                    }
                    const token = res.data.token;
                    this.guardarUsuario(token);
                    console.log('Usuario DB: ',this.usuarioDB);
                })
                .catch(e =>{
                    console.log(e)
                })
            this.cargando = false;
        },
        showNotification(text, color) {
            this.notification.snackbar = true;
            this.notification.text = text
            this.notification.color = color
        },
    },

}
</script>

<style>

</style>