<template>
    <div id="conteneur">
        <div id="accueil">
            <div>
                <i class="fas fa-user-friends"></i>
                <p>Apprenez à connaître votre entourage professionnel.</p>
            </div>
            <div>
                <i class="fas fa-image"></i>
                <p>Partagez vos expériences.</p>
            </div>
            <div>
                <i class="far fa-comments"></i>
                <p>Rejoignez la conversation.</p>
            </div>
        </div>
        <div id="signup">
            <div >
                <div>
                    <div id="title">
                        <h3>Inscription</h3>
                    </div>
                    <div id="lien">
                        <router-link to="/login">Connexion</router-link>
                    </div>
                </div>
            </div>
            <div>
                <div class="register">
                    <input type="text" id="firstName" placeholder="Prénom" aria-label="Prénom de l'utilisateur" v-model="dataUser.firstName"/>
                    <input type="text" id="lastName" placeholder="Nom" aria-label="Nom de famille de l'utilisateur" v-model="dataUser.lastName"/>
                    <input type="email" id="email" placeholder="name@exemple.com" v-model="dataUser.email"/>
                    <input type="password" id="password-input" placeholder="Votre mot de passe" aria-label="Mot de passe de l'utilisateur" v-model="dataUser.password" 
                    v-on:keyup.enter="submitSignup"/>
                    <button type="submit" aria-label="Inscription de l'utilisateur" @click.prevent="submitSignup" class="btn">S'inscrire</button>
                </div>
            </div>
        </div>
    </div>
</template>


<script>
import axios from "axios";
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
                axios
                    .post("http://localhost:3000/api/users/signup",
                        this.dataUser
                    )
                    .then(response => {
                        console.log(response);
                        this.$router.push("/login");
                    })
                    .catch(error => {
                        console.log(error.response)
                    })
            } else {
                alert("L'un des champs n'est pas renseigné !");
            }
        },
    },
};
</script>

<style>
    body, * {
        margin: 0;
        padding: 0;
        font-family: 'Texturina', serif;
    }
    #conteneur {
        display: flex;
        justify-content: center;
        margin: 100px;
    }
    #signup {
      text-align: center;
      margin-left: 80px;
    }
    #title h3 {
        padding: 10px 0;
        font-size: 3rem;
    }
    #lien a {
        text-decoration: none;
        color: black;
        font-size: 1rem;
    }
    #lien a:hover{
        color: #FD2D01;
    }
    .register{
        display: flex;
        flex-direction: column;
        align-items: center;
        padding: 50px 50px 0 50px;
    }
    .register input {
        margin-bottom: 30px;
        font-size: 25px;
    }
    #accueil {
        background-image: url(../assets/images/icon.png);
        background-repeat: no-repeat;
        background-position: 50% 50%;
        text-align: center;
        display: flex;
        flex-direction: column;
        padding: 165px 80px;
    }
    #accueil div i {
        float: left;
        font-size: 35px;
        padding-top: 10px;
    }
    #accueil div p {
        font-size: 2rem;
    }
    #accueil div {
        padding: 10px;
    }
    .btn {
        width: 50%;
        padding: 5px;
        border-radius: 15px;
        font-size: 1.2rem;
        margin-top: 50px;
        background-image: linear-gradient(to top left,rgba(0, 0, 0, .2),rgba(0, 0, 0, .2) 30%,rgba(0, 0, 0, 0));       
    }
    .btn:hover {
        background-image: linear-gradient(to top left,rgba(0, 0, 0, .2),rgba(0, 0, 0, .2) 30%,rgba(0, 0, 0, 0));
        background-color: #FD2D01;
    }
    @media screen and (max-width: 568px){
        #conteneur {
            display: flex;
            flex-direction: column;
            margin: 10px 0 0 0;
            width: 100%;
        }
        #accueil {
            padding: 0;
            align-items: center;
            background-size: 160px;
        }
        #accueil div p {
            font-size: 0.9rem;
            margin-bottom: 10px;
        }
        #accueil div i {
            padding: 0 0 0 10px;
            font-size: 25px;
        }
        #accueil div {
            padding: 0;
            width: 100%;
        }
        #signup {
            padding: 0;
            margin: 0;
        }
        .register {
            padding: 5px 0 0 0;
        }
        .register input {
            margin-bottom: 20px;
            font-size: 15px;
        }
        .btn {
            margin-top: 0;
            font-size: 15px;
            padding: 3px;
        }
        #title h3 {
            font-size: 2rem;
            padding: 20px 0 5px 0;
        }
    }
</style>