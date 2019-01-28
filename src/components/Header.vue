<template>
    <header>
        <nav class="navbar navbar-expand-sm">
            <a class="navbar-brand" href="#">HelloWorld SNCF</a>
            <button class="navbar-toggler d-lg-none" type="button" data-toggle="collapse" data-target="#collapsibleNavId" aria-controls="collapsibleNavId"
                aria-expanded="false" aria-label="Toggle navigation">
                <span class="navbar-toggler-icon"></span>
            </button>
            <div class="collapse navbar-collapse" id="collapsibleNavId">
                <ul class="navbar-nav mr-auto mt-2 mt-lg-0">
                    <li class="nav-item active">
                        <a class="nav-link" href="#">Home <span class="sr-only">(current)</span></a>
                    </li>
                </ul>
            </div>
        </nav>
    </header>
</template>

<script>
export default {
    data() {
        return {
            
        }
    }
}
</script>

<style scoped>

.navbar {
    background-color: #009BD7;
}

.navbar * {
    color: white;
}

.nav-link:hover {
    background-color: whitesmoke;
    color: black;
    border-radius: 50px;
}
    
</style>