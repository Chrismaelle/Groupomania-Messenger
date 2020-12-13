<template>
    <div id="nav">
        <div class="logo">
            <router-link to="/"><img src="../assets/images/logo-groupomania.png" id="logo-groupomania"></router-link>
        </div>
        <div id="nav-info">
            <div v-if="$route.path==='/login' || $route.path==='/signup' ? false : true" v-bind="account">
                <div role="link" aria-label="Accès aux informations utilisateurs" class="navbar-user">
                    <img v-bind:src="userPhoto">
                    <div>
                        <ul id="list">
                            <li><router-link to="/account" class="menu">Informations</router-link></li>
                            <li v-if="admin == 1"><router-link to="/allusersadmin" class="menu">Tous les utilisateurs</router-link></li>
                            <li><a href="#" class="logOut menu" @click="logOut()">Se déconnecter</a></li>
                      </ul>
                  </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import axios from 'axios';
export default {
    name: 'NavBar',
    data() {
        return {
          cookie: this.$cookies.get("token"),
          userPhoto: '',
          admin: '',
          account: ''
        }
    },
    created() {
        if (this.cookie == null) {
            return false;
        } else {
            axios
                .get('http://localhost:3000/api/users/me', {
                    headers: { 
                    Authorization: "Bearer " + this.cookie }
                })
                .then(response => {
                    this.userPhoto = response.data.user.photo;
                    this.admin = response.data.user.permission;
                })
                .catch(error => {
                    console.log(error);
                })
        }
    },
    methods: {
        logOut() {
          this.$confirm(
            {
                message: `Êtes-vous sûr de vous déconnecter ?`,
                button: {
                    no: 'No',
                    yes: 'Yes'
                },
                callback: confirm => {
                    if (confirm) {
                      this.$cookies.remove("token");
                      this.$router.go();
                    }
                }
            })
        },
        loadUser() {
            if (this.$cookies.isKey("token")) {
                axios
                    .get('http://localhost:3000/api/users/me', {
                        headers: { 
                        Authorization: "Bearer " + this.cookie }
                    })
                    .then(response => {
                        this.userPhoto = response.data.user.photo;
                        this.admin = response.data.user.permission;
                    })
                    .catch(error => {
                        console.log(error);
                    })
            } else {
                return false;
            }
        }
    },
    watch: {
        '$route' () {
            this.cookie = this.$cookies.get("token");
            this.loadUser();
        }
    }
}
</script>

<style>
    body, * {
        margin: 0;
        padding: 0;
        font-family: 'Texturina', serif;
    }
    #nav {
        text-align: center;
    }
    #logo-groupomania {
        max-width: 350px;
        max-height: 200px;
        width: 100%;
        height: 100%;
        margin-left: 50px;
    }
    .navbar-user img {
        max-width: 40px;
        max-height: 40px;
        height: 100%;
        width: 100%;
        border-radius: 50px;
        box-shadow: 2px 2px 2px #FDD3D3;
        margin: 50px 70px 0 0;
    }
    .navbar-user {
        display: flex;
        flex-direction: column;
        align-items: flex-end;
        background-image: url(../assets/images/fond.jpg);
        background-repeat: no-repeat;
        height: 300px;
    }
    .menu {
        text-decoration: none;
        color: white;
        margin-right: 35px;
    }
    #list {
        display: flex;
        flex-direction: column;
        line-height: 50px;
    }
    #list li {
        list-style: none;
    }
    .logo {
        background-color: white;
    } 
    @media screen and (max-width: 568px) {
        #logo-groupomania {
            margin: 0px;
        }
    }
</style>