<template>
    <div>
        <div class="list-group" v-for="list in repolist" :key="list.id">
            <a href="list.html_url" class="list-group-item list-group-item-action active" title="list.description">
                {{list.full_name}}
            </a>
            <span class="badge badge-pill badge-primary">{{ list.forks }}</span>
           
        </div>
        <!-- TODO: Crear componente GitHubRepos -->
    </div>
</template>

<script>
export default {
    name: 'GitHubRepos',
    props: [ "repolist"
        // TODO: crear parámetro 'repolist'
    ],
}
</script>

<style scoped>
</style>