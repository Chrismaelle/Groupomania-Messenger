<template>
    <div id="nav">
        <div>
            <router-link to="/"><img src="../assets/images/logo-groupomania.png" id="logo-groupomania"></router-link>
        </div>
        <div>
            <div v-if="$route.path==='/login' || $route.path==='/signup' ? false : true" v-bind="account">
                <div role="link" aria-label="Accès aux informations utilisateurs">
                    <img v-bind:src="userPhoto">
                    <div>
                        <ul>
                            <li><router-link to="/account">Informations</router-link></li>
                            <li v-if="admin == 1"><router-link to="/allusersadmin">Tous les utilisateurs</router-link></li>
                            <li><a href="#" class="logOut" @click="logOut()">Se déconnecter</a></li>
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
    }
    #nav {
        text-align: center;
    }
    #logo-groupomania {
        max-width: 400px;
        max-height: 250px;
        width: 100%;
        height: 100%;
    }

</style>