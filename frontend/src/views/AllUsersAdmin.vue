<template>
    <div id="all-users">
        <h1 class="title">Gestion des utilisateurs</h1>
        <p>Vous pouvez modifier la permission des utilisateurs en changeant le false à true puis en mettant à jour.</p>
        <p>Attention ! Une fois mis à jour vous n’aurez plus accès à l’utilisateur car il sera admin à son tour.</p>
        <p>Vous pouvez aussi supprimer un utilisateur.</p> 
        <div class="all-user-items">
            <UserItem v-for="user in allUsers" :photo="user.photo" :firstName="user.firstName" :lastName="user.lastName" :email="user.email"
            v-on:user-deleted="deleteUser(user.id)" v-on:user-updated="updateUser(user.id, user)" :key="user.id">
            <template v-slot:Permission>
                <select name="permission" id="permission-select" v-model.number="user.permission">
                    <option v-for="option in options" v-bind:key="option.value">
                        {{ option.text }}
                    </option>
                </select>
            </template>
            </UserItem>
        </div>
    </div>
</template>



<script>
import UserItem from '@/components/UserItem.vue'
import axios from 'axios'
export default {
    name: 'allUsersAdmin',
    components: {
        UserItem
    },
    data() {
        return {
            cookie: this.$cookies.get("token"),
            allUsers: [],
            user: {
                firstName: 'Victor',
                lastName: 'Deweerdt',
                photo: '',
                email: '',
                permission: false,
            },
            options: [
                { text: 'false', value: '0' },
                { text: 'true', value: '1' }
            ],
        }
    },
    created() {
        axios
            .get('http://localhost:3000/api/admin/users', {
                headers: { Authorization: "Bearer " + this.cookie }
            })
            .then(response => {
                console.log("users", response.data);
                this.allUsers = response.data;
                })
            .catch(error => {
                console.log(error);
            });
    },
    methods: {
        deleteUser(id) {
            this.$confirm(
                {
                    message: `Êtes-vous sur de vouloir supprimer ce compte ?`,
                    button: {
                        no: 'No',
                        yes: 'Yes'
                    },
                    callback: confirm => {
                        if (confirm) {
                            axios
                                .delete('http://localhost:3000/api/admin/users/' + id, {
                                    headers: { Authorization: "Bearer " + this.cookie }
                                })
                                .then(() => console.log({message : 'Utilisateur supprimé !'}))
                                .catch(error => console.log(error))
                            this.$router.go();
                        }
                    }
                }
            )
        },
        updateUser(id, user) {
            this.$confirm(
                {
                    message: `Attention ! Si vous modifiez la permission de cet utilisateur, vous ne pourrez plus y accéder. Les admins n'ont accès qu'aux non-admins. Continuer ?`,
                    button: {
                        no: 'No',
                        yes: 'Yes'
                    },
                    callback: confirm => {
                        if (confirm) {
                            console.log(user);
                                axios
                                    .put('http://localhost:3000/api/admin/users/' + id, user, {
                                        headers: { 
                                            Authorization: "Bearer " + this.cookie }
                                    })
                                    .then(() => console.log({message : 'Utilisateur modifié !'}))
                                    .catch(error => console.log(error))
                                this.$router.go();
                            }
                    }
                }
            )
        }
    }
}
</script>

<style>
    .all-user-items {
        background-color: #c7c7c7;
        border-radius: 10px;
        width: 90%;
        margin: 30px 15px 15px 730px;
        width: 450px;
    }
    .title {
        text-align: center;
        margin-bottom: 20px;
    }
    #all-users p {
        text-align: center;
        margin: 0 10px;
    }
    @media screen and (max-width: 568px) {
        .title {
        text-align: center;
        font-size: 1.7rem;
        margin-bottom: 20px;
        }
        #all-users p {
            text-align: justify;
            margin: 0 10px;
        }
        .all-user-items {
            background-color:#c7c7c7;
            border-radius: 10px;
            width: 90%;
            margin: 15px;
        }
        #permission-select {
            font-size: 15px;
        }
    }
    
</style>