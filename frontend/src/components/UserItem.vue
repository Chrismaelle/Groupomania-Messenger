<template>
    <div class="user-item">
        <div class="main-side">
            <div class="left-side">
                <img class="user-photo" :src="photo">
                <div class="user-identity">
                    <p class="user-name">{{ firstName }} {{ lastName }}</p>
                    <p class="email">{{ email }}</p>
                </div>
            </div>
            <div class="right-side">
                <button 
                    id="update-btn"
                    type="submit" 
                    class="btn btn-primary mb-2 btn-update"
                    aria-label="Modification de permission admin d'un utilisateur"
                    v-on:click.prevent="updateUsers(user)"
                    >Mettre à jour
                </button>
                <slot name="Permission"></slot>
            </div>
        </div>
        <div class="secondary-side">
            <button 
                class="delete-user"
                aria-label="Suppression d'un utilisateur" 
                v-on:click.prevent="deleteUser(user)"><span class="mdi mdi-delete-outline"></span></button>
        </div>
    </div>
</template>


<script>
export default {
    name: 'UserItem',
    props: {
        firstName: {
            type: String,
            default: 'Victor'
        },
        lastName: {
            type: String,
            default: 'Deweerdt'
        },
        photo: {
            type: String,
            default: 'http://localhost:3000/images/DSC04282.jpg1604754621573.jpg'
        },
        email: {
            type: String,
            default: 'vdeweerdt@groupomania.com'
        },
        user: {
            type: String
        }
    },
    methods: {
        deleteUser(user) {
            this.$emit('user-deleted', user);
        },
        updateUsers(user) {
            this.$emit('user-updated', user);
        }
    }
}
</script>
