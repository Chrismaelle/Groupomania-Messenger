<template>
    <div id="signup">
        <div>
            <div>
                <div>
                    <p> S'inscrire</p>
                </div>
                <div>
                    <a href="/login">Se connecter</a>
                </div>
            </div>
        </div>
        <div>
            <div>
                <input type="text" id="firstName" placeholder="First-name" aria-label="Prénom de l'utilisateur" v-model="dataUser.firstName">
                <input type="text" id="lastName" placeholder="Last-name" aria-label="Nom de l'utilisateur" v-model="dataUser.lastName">
                <input type="email" id="email" placeholder="name@exemple.com" v-model="dataUser.email">
                <input type="password" id="password-input" placeholder="Votre mot de passe" aria-label="Mot de passe de l'utilisateur" v-model="dataUser.password" v-on:keyup.enter="submitSignup">
                <button type="submit" aria-label="Inscription de l'utilisateur" @click.prevent="submitSignup"></button>
            </div>
        </div>
    </div>
</template>

<script>
import axios from 'axios'

export default {
    name: 'signup',
    data() {
        return {
            dataUser: {
                firstName: '',
                lastName: '',
                email: '',
                password: ''
            }
        };
    },
    methods: {
        submitSignup() {
            if (this.firstName !== null || this.lastName !== null || this.email !== null || this.password !== null) {
                axios.post("http://localhost:3000/api/users/signup", this.dataUser)
                .then(response => {
                    console.log(response);
                    this.$router.push("/login");
                })
                .catch(error => {
                    console.log(error.response)
                })
            } else {
                alert("L'un des champs n'est pas renseigné !")
            }
        },
    },
};
</script>