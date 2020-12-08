<template>
  <div class="account">
    <div class="user">
      <img id="user-photo" v-bind:src="dataUser.photo">
    </div>
    <div class="fields c-12">
                <input 
                    type="text" 
                    class="input-l" 
                    id="firstName" 
                    placeholder="First-name"
                    aria-label="Prénom de l'utilisateur"
                    v-model="dataUser.firstName"
                />
                <input 
                    type="text" 
                    class="input-l" 
                    id="lastName" 
                    placeholder="Last-name"
                    aria-label="Nom de famille de l'utilisateur"
                    v-model="dataUser.lastName"
                />
                <input 
                    type="email" 
                    class="input-l" 
                    id="email"
                    aria-label="Email de l'utilisateur"
                    placeholder="name@example.com"
                    v-model="dataUser.email"
                />

                <label class="file-select c-12">
                    <div class="select-button" role="button" aria-label="Téléchargement d'un fichier">
                        <span v-if="newUserData.photo">Fichier séléctionné : {{newUserData.photo.name}}</span>
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

                <div class="buttons">
                  <div class="c-12">
                    <button 
                      type="submit" 
                      class="btn btn-primary mb-2 btn-update"
                      aria-label="Modifier les informations utilisateur"
                      @click.prevent="updateUserInformations"
                    >Mettre à jour les informations</button>
                  </div>
                  <div class="c-12">
                    <button 
                      type="submit" 
                      class="btn btn-primary mb-2 btn-delete"
                      aria-label="Supprimer l'utilisateur"
                      @click.prevent="deleteUser"
                    >Supprimer le compte</button>
                  </div>
                </div>
            </div>
  </div>
</template>


<script>
import axios from "axios";
export default {
    name: 'account',  
    data() {
        return {
          cookie: this.$cookies.get("token"),
          dataUser: {
              firstName: '',
              lastName: '',
              email: '',
              photo: ''
          },
          newUserData: {
            photo: null
          }
        };
    },
    created() {
      axios
        .get("http://localhost:3000/api/users/me", {
          headers: { Authorization: "Bearer " + this.cookie }
        })
        .then(response => {
          console.log(response.data);
          this.dataUser = response.data.user;
        })
    },
    methods: {
        updateUserInformations() {
          console.log(this.newUserData.photo);
          if (this.newUserData.photo !== null) {
            console.log('non null');
            let formData = new FormData();
            formData.append('email', this.dataUser.email);
            formData.append('firstName', this.dataUser.firstName);
            formData.append('lastName', this.dataUser.lastName);
            formData.append('photo', this.newUserData.photo, this.newUserData.photo.name);
            axios
              .put('http://localhost:3000/api/users/me',
                    formData,
                    {
                    headers: {
                        'Content-Type': 'multipart/form-data',
                        Authorization: "Bearer " + this.cookie
                    },
                  }
                ).then(() => {
                  console.log('Image téléchargée !');
                  this.$router.go();
              })
              .catch(error =>
                console.log(error)
              );
            } else {
              console.log('null');
              let formData = new FormData();
              formData.append('email', this.dataUser.email);
              formData.append('firstName', this.dataUser.firstName);
              formData.append('lastName', this.dataUser.lastName);
              formData.append('photo', this.dataUser.photo);
              axios
                .put('http://localhost:3000/api/users/me',
                      formData,
                      {
                      headers: {
                          'Content-Type': 'multipart/form-data',
                          Authorization: "Bearer " + this.cookie
                      },
                    }
                  ).then(() => {
                    console.log('Image téléchargée !');
                    this.$router.go();
                })
                .catch(error =>
                  console.log(error)
                );
            }
          },
        loadUser() {
          axios
            .get("http://localhost:3000/api/users/me", {
              headers: { Authorization: "Bearer " + this.cookie }
            })
            .then(response => {
              console.log(response.data);
              this.dataUser = response.data.user;
            })
        },
        handleFileUpload(){
          this.newUserData.photo = this.$refs.photo.files[0];
        },
        deleteUser() {
            this.$confirm(
                {
                    message: `Êtes-vous sur de vouloir supprimer votre compte ?`,
                    button: {
                        no: 'No',
                        yes: 'Yes'
                    },
                    callback: confirm => {
                        if (confirm) {
                        const id = this.dataUser.id;
                        axios
                            .delete('http://localhost:3000/api/users/' + id, {
                            headers: { Authorization: "Bearer " + this.cookie }
                            })
                            .then(response => {
                            console.log(response.data);
                            this.$cookies.remove("token");
                            this.$router.push("/signup");
                            })
                            .catch(error => console.log(error))
                        }
                    }
                }
            )
        },
    },
};
</script>