<template>
    <div class="new-post c-6 cm-12">
        <div class="identity">
        <img id="user-photo" :src="user.photo">
        <div>
            <p id="user-name">{{ user.firstName }} {{ user.lastName }}</p>
        </div>
        </div>
        <form @submit.prevent="createPost">
        <div class="post-textarea">
            <textarea 
                id="post-area" 
                class="form-control"
                aria-label="Contenu texte d'un post" 
                v-model="postContent.content" 
                placeholder="Écrire votre post ici"
            ></textarea>
        </div>
        <div class="new-post-buttons">
            <label class="c-6 cm-12 file-select">
                <div class="select-button" role="button" aria-label="Téléchargement d'un fichier">
                    <span v-if="postContent.attachments">Fichier séléctionné : {{postContent.attachments.name}}</span>
                    <span v-else class="add-file"><span class="mdi mdi-image"></span>Ajouter un fichier</span>
                </div>
                <input 
                    type="file"
                    id="photo"
                    class="upload-input"
                    ref="photo" 
                    v-on:change="handleFileUpload()"
                />
            </label>
            <div class="c-6 cm-12">
                <input type="submit" id="btn-new-post" class="btn-lg btn-l" value="Publier" aria-label="Publication d'un post" v-on:click="loadAllPosts()"/>
            </div>
        </div>
        </form>
    </div>

</template>

<script>
import axios from 'axios'
export default {
    name: 'NewPost',
    props: {
        submit: Function
    },
    data() {
        return {
            cookie: this.$cookies.get("token"),
            user: "",
            userPhoto: '',
            postContent: {
                content: null,
                attachments: null,
            }
        }
    },
    created() {
        axios
        .get('http://localhost:3000/api/users/me', {
            headers: { 
            Authorization: "Bearer " + this.cookie }
        })
        .then((response) => {
            this.user = response.data.user;
        })
        .catch(error => {
            console.log(error);
        })
    },
    methods: {
        
        createPost() {
            if (this.postContent.content !== null) {
                if (this.postContent.attachments !== null) {
                    let formData = new FormData();
                    formData.append('content', this.postContent.content);
                    formData.append('attachments', this.postContent.attachments, this.postContent.attachments.name);
                    console.log('content with attachments')
                    axios
                        .post('http://localhost:3000/api/posts', 
                            formData , {
                            headers: {
                            'Content-Type': 'multipart/form-data',
                            Authorization: "Bearer " + this.cookie }
                            }
                        )
                        .then(response => {
                            console.log(response);
                            })
                        .catch(error => {
                            console.log(error)
                            });
                } else {
                    let formData = new FormData();
                    formData.append('content', this.postContent.content);
                    console.log('content')
                    axios
                        .post('http://localhost:3000/api/posts', 
                            formData , {
                            headers: {
                            'Content-Type': 'multipart/form-data',
                            Authorization: "Bearer " + this.cookie }
                            }
                        )
                        .then(response => {
                            console.log(response);
                            })
                        .catch(error => {
                            console.log(error)
                            });
                }
            } else {
                return false;
            }
        },
        uploadImage() {
            console.log('Image Téléchargée !')
        },
        loadAllPosts() {
            this.$emit('load-all-posts');
        },
        handleFileUpload(){
        this.postContent.attachments = this.$refs.photo.files[0];
        },
    },
}
</script>