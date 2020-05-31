<template>
    <div class="header has-background-dark flex">
        <div class="animated zoomIn flex-item left">
        <h1 class="title has-text-white">My Covid-19 Tracker <small>v2.0</small></h1>
        </div>
        <div class="flex-item">
            <button class="button is-light right"><a href="https://github.com/slaytherin1/my-covid-app-tracker"><i class="fa fa-github" aria-hidden="true"></i> Github</a> </button>
        </div>
    </div>
</template>
<script>
export default {
    name: 'Header'
}
</script>
<style scoped>
.flex {
    display: flex;
    align-self: center;
    padding: 1%;
    flex-wrap: wrap;
    align-content: center;
}
small {
    font-size: 10px;
}

.flex-item {
    flex-basis: 100px;
}

.left {
    flex-grow: 1;
}

.right {

}

</style>