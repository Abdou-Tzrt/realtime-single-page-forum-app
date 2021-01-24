<template>
    <v-container>
        <v-form @submit.prevent="signup">
            <v-text-field
                v-model="form.name"
                label="Name"
                type="text"
                required
            ></v-text-field>
            <span v-if="errors.name">error name</span>

            <v-text-field
                v-model="form.email"
                label="E-mail"
                type="email"
                required
            ></v-text-field>
            <span v-if="errors.email">error email</span>

            <v-text-field
                v-model="form.password"
                label="Password"
                type="password"
                required
            ></v-text-field>
            <span  v-if="errors.password">error password</span>

            <v-text-field
                v-model="form.password_confirmation"
                label="Password"
                type="password"
                required
            ></v-text-field>

            <v-btn
                type="submit"
                style="background: green;color: white"
                class="mr-4"
            >
                Signup
            </v-btn>

            <router-link to="/login">
                <v-btn
                    color="blue"
                    class="mr-4">Login</v-btn>
            </router-link>

        </v-form>
    </v-container>
</template>

<script>
    export default {
        data(){
            return {
                form : {
                    name:null,
                    email:null,
                    password:null,
                    password_confirmation:null
                },
                errors:{}
            }
        },
        created(){
            if(User.loggedIn()){
                this.$router.push({name:'forum'})
            }
        },
        methods:{
            signup(){
                axios.post('/api/auth/signup',this.form)
                    .then(res => {
                        User.responseAfterLogin(res)
                        this.$router.push({name:'forum'})
                    })
                    .catch(error => this.errors = error.response.data)
            }
        }
    }
</script>

<style>

</style>
