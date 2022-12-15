<template>
    <div class="page-my-account">
        <div class="columns is-multiline">
            <div class="column is-12">
                <h1 class="title">My account</h1>
            </div>

            <div class="field has-addons mt-6">
                    <div class="field">
                        <div class="control">
                           <input type="email" class="input" placeholder="user@gmail.com" v-model="email">
                        </div>
                        <div class="control">
                           <input type="date" class="input" v-model="dob">
                        </div>
                        <div class="control">
                        <button class="button is-dark" @click="submitForm">Update</button>
                    </div>
                    </div>

                </div>

                

            <div class="column is-12">
                <button @click="logout()" class="button is-danger">Log out</button>
            </div>

            <hr>

            
        </div>
    </div>
</template>

<script>
import axios from 'axios'

export default {
    name: 'MyAccount',
    components: {
       
    },
    data() {
        return {
            email: '',
            dob: ''
        }
    },
    mounted() {
        document.title = 'My account | Djackets'
    },
    methods: {
        logout() {
            axios.defaults.headers.common["Authorization"] = ""
            localStorage.removeItem("token")
            localStorage.removeItem("username")
            localStorage.removeItem("userid")
            this.$store.commit('removeToken')
            this.$router.push('/')
        },

        submitForm() {
            this.$store.commit('setIsLoading', true)
            this.profileHandler()
        },

        async profileHandler() {

            const data = {
                'email': this.email,
                'dob': this.dob
            }

            await axios
                .post('/api/v1/profile/', data)
                .then(response => {
                    this.$router.push('/')
                })
                .catch(error => {
                    console.log(error)
                })

        }

        
        
    }
}
</script>