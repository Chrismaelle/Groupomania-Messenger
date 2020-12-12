<template>
    <div>
        <div>
            <div>
                <img :src="photo">
                <div>
                    <p>{{ firstName }} {{ lastName }}</p>
                    <p>{{ email }}</p>
                </div>
            </div>
            <div >
                <button type="submit" aria-label="Modification de permission admin d'un utilisateur" v-on:click.prevent="updateUsers(user)">Mettre à jour
                </button>
                <slot name="Permission"></slot>
            </div>
        </div>
        <div>
            <button aria-label="Suppression d'un utilisateur" v-on:click.prevent="deleteUser(user)"></button>
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
