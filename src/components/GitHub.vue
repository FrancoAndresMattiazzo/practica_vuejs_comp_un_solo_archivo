<template>
   <div class="container mx-auto">
        <input type="text" id="nombre" class="form-control" @keydown.enter = "obtenerUsuario">

        <div class="alert alert-danger mt-5" v-if="advertencia">
        El usuario no existe
        </div>

        <div v-else>
            <div class="card col-xl-3 col-lg-5 col-md-7 col-sm-9 col-10 mx-auto" v-if="card">
                <img :src="user.avatar_url" class="card-img-top" alt="...">
                <div class="card-body">
                    <h5 class="card-title text-center">{{ user.login }}</h5>
                    <button class="btn btn-primary mx-auto" @click = "obtenerRepositorios">Repositorios</button>
                    <a :href="'https://github.com/' + user.login" target="_blank" class="btn btn-light mx-auto mt-2">Url GitHub</a>
                </div>
            </div>

            <div v-if="mostrarRepo"  class="container row">
                <div class="w-50">
                    <div class="card col-sm" v-if="card2">
                        <img :src="user.avatar_url" class="card-img-top" alt="...">
                        <div class="card-body">
                        <h5 class="card-title text-center">{{ user.login }}</h5>
                        <button class="btn btn-primary mx-auto" @click = "obtenerRepositorios">Repositorios</button>
                        <a :href="'https://github.com/' + user.login" target="_blank" class="btn btn-light mx-auto mt-2">Url GitHub</a>
                        </div>
                    </div>
                </div>
                <div class="contianer w-50">
                    <div v-for="repo in repos" :key="repo.id" class="card">
                        <div class="card-body">
                            <p class="card-title">{{ repo.name }}</p>
                            <p class="card-text">{{ repo.description }}</p>
                            <a :href="repo.html_url" target="_blank" class="btn btn-primary mt-2">{{ repo.forks }}</a>
                        </div>
                    </div>
                </div>
            </div>
        </div>
        <GitHubRepos />
    </div>
</template>

<script>
// TODO: Importar componente GitHubRepos
import GitHubRepos from './GitHubRepos.vue'
export default {
    name: 'GitHub',
    components: {
        GitHubRepos
        // TODO: Importar componente GitHubRepos
    },
    data: function() {
        return {
            user: null,
            mostrarRepo: false,
            advertencia: false,
            card: false,
            repos: [],
            
            
            // TODO: crear variables de datos para el funcionamiento del componente
        }
    },
    methods: {
        obtenerUsuario: function() {
            // TODO: Función para obtener los datos de usuario de la API de GitHub
            // TODO: Añadir lógica para resetear los cambios en el interfaz: desactivar campo de envío,
            // resetear mensaje de error, mostrar lista de repositorios,...
            // Obtener datos de autenticación de usuario para hacer peticiones
            // autenticadas a la API de GitHub
            // var userAuth = process.env.VUE_APP_USERNAME || "FrancoAndresMattiazzo";
           // var passAuth =  process.env.VUE_APP_USERTOKEN || "pass";
            // TODO: realizar petición fetch par obtener los datos y mostrar la información en la página
            // Ejemplo de paso de datos de autorización con fetch: https://stackoverflow.com/questions/43842793/basic-authentication-with-fetch
        fetch(`https://api.github.com/users/${document.getElementById("nombre").value}`)
        .then((response) => {
          if (!response.ok) throw new Error("Error");
          this.advertencia = true;
          return response.json();
        })
        .then((data) => {
          this.user = data;
          this.advertencia = false;
          this.card = true;
          this.mostrarRepo = false;
          this.repos = [];
        })
        .catch(() => {
          this.advertencia = true;
          this.card = false;
          this.mostrarRepo = false;
        });
    },
        obtenerRepositorios: function() {
            // TODO: Función para obtener los repositorios del usuario desde la API de GítHub
            // La URL de los repositorios de usuario se puede obtener a través del campo 'repos_url' de los datos del usuario
            // Obtener datos de autenticación de usuario para hacer peticiones
            // autenticadas a la API de GitHub
           // var userAuth = process.env.VUE_APP_USERNAME || "FrancoAndresMattiazzo";
           // var passAuth = process.env.VUE_APP_USERTOKEN || "pass";
            // TODO: realizar petición fetch par obtener los datos y mostrar la información en la página
            // Ejemplo de paso de datos de autorización con fetch: https://stackoverflow.com/questions/43842793/basic-authentication-with-fetch
    
            //headers.append('Content-Type', 'text/json');
            fetch(`https://api.github.com/users/${this.user.login}/repos`)
        .then((response) => {
          if (!response.ok) throw new Error("Error");
          return response.json();
        })
        .then((data) => {
          this.repos = data;
          this.card2 = true;
          this.card = false;
          this.mostrarRepo = true;
        })
        .catch((error) => {
            console.log(error);
    });
    },
  },
};
</script>

<style scoped>
</style>