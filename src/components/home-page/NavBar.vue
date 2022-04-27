<template>
    <nav class="navbar navbar-expand-lg navbar-light bg-light w-100 shadow-sm">
        <div class="container-fluid">
            <a class="navbar-brand" href="/">
                <img src="@/assets/logo_devox.png">
            </a>
            <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarNavAltMarkup" aria-controls="navbarNavAltMarkup" aria-expanded="false" aria-label="Toggle navigation">
                <span class="navbar-toggler-icon"></span>
            </button>
            <div class="collapse navbar-collapse" id="navbarNavAltMarkup">
                <div class="navbar-nav">
                    <a class="nav-link rounded" href="/ajouter/projet">Nouveau Projet</a>
                    <a class="nav-link rounded" href="/clients">Liste des Clients</a>
                </div>
            </div>
        </div>
    </nav>
</template>

<script>
export default {

}
</script>

<style>
    .navbar-brand img{
        width: 120px;
        height: 70px;
    }
    .nav-link:hover{
        background:rgb(239, 107, 41);
        color:white !important;
        font-size: bolder !important;
    }
</style>