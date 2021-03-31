<template>
    <v-form
        ref="form"
        v-model="valid"
        lazy-validation
        class="mt-5"
    >
        <v-progress-linear
            :active="loading"
            :indeterminate="loading"
            absolute
            top
            color="cyan"
        ></v-progress-linear>
        <v-text-field
            outlined
            v-model="user.name"            
            :rules="nameRules"
            label="Name"
            required
            prepend-inner-icon="fas fa-user"            
        ></v-text-field>

        <v-text-field
            outlined
            v-model="user.email"
            :rules="emailRules"
            label="Email"
            required
            prepend-inner-icon="fas fa-at"              
        >
        </v-text-field>

        <v-text-field
            outlined
            v-model="user.password"   
            :rules="passwordRules"         
            label="Password"
            required
            prepend-inner-icon="fas fa-lock"
            type="password"            
        >            
        </v-text-field>

        <v-text-field
            outlined            
            label="Confirmar Password"
            v-model="user.password2"
            :rules="passwordRules2"
            required
            prepend-inner-icon="fas fa-lock"
            type="password"            
        >            
        </v-text-field>

        <v-btn
            :disabled="!valid"            
            color="blue darken-3"            
            class="mr-4 white--text"
            @click="register()"
        >
            Registrar
        </v-btn>

        <v-btn            
            color="grey"
            dark
            @click="resetRegister()"
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
export default {
    data() {
        return {
            valid:true,
            valid2:true,
            loading:false,
            user:{
                name:'',
                email:'',
                password:'',
                password2:''
            },    
            
            notification:{
                snackbar: false,
                text: '',
                timeout: 3000,
                color:''
            },
            
            
            nameRules: [
                v => !!v || 'Name is required',
                v => (v && v.length <= 50) || 'Name must be less than 50 characters',
            ],
            emailRules: [
                v => !!v || 'Email is required',
                v => /.+@.+\..+/.test(v) || 'Email must be valid',
            ],
            passwordRules: [
                v => !!v || 'Password is required',
                v => (v && v.length > 3) || 'Passowwrd must be more than 3 characters',
            ],

            passwordRules2: [
                v => !!v || 'Confirma Password is required',
                v => (v && v.length > 3) || 'Password must be more than 3 characters',                
            ],

        }
    },
    

    methods: {

        showNotification(text, color) {
            this.notification.snackbar = true;
            this.notification.text = text
            this.notification.color = color
        },

        confirmPassword(){
            let mensaje = ''
            if (this.user.password.length == 0) {
                mensaje = 'Password es obligatorio.'
            } else if (this.user.password2.length == 0) {
                mensaje = 'Debe confirmar password'
            } else if (this.user.password != this.user.password2){
                mensaje = 'Passwords no coinciden.'
            }

            return mensaje
        },

        register() {            
            this.loading = true;
            console.log('Uusuario:',this.user)           
            let confirmarPassword = ''

            confirmarPassword = this.confirmPassword()

            if (confirmarPassword != '') {
                this.showNotification(confirmarPassword,'error')
                this.loading = false
                return
            }            
            
            this.axios.post('/users',this.user)
                .then(res => {
                    if (res.data === 'Usuario ya existente') {
                        this.showNotification('Usuario ya existe','error')
                        this.loading = false;
                        return 
                    }
                    this.showNotification('Registrado exitosamente.','success')
                    this.resetRegister();
                    console.log(res)
                    this.loading = false;
                })
                .catch(e =>{
                    console.log('FrontEnd ',e.response);
                    this.showNotification(e.response.data.message,'error')                    
                    this.loading = false;

                })
        },

        resetRegister(){
            this.user.name = '';
            this.user.email = '';
            this.user.password = '';
            this.user.password2 = '';
            this.$refs.form.resetValidation();
        },

        resetValidation () {
            this.$refs.form.resetValidation()
        },

    },
}
</script>

<style>

</style>