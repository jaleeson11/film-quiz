<template>
    <div>
        <b-navbar class="mb-3">
            <b-navbar-brand>
                {{ logo }}
            </b-navbar-brand>
        </b-navbar>
    </div>
</template>

<script>
export default {
    props: {
        logo: String
    }
}
</script>

<style scoped>
    @import url('https://fonts.googleapis.com/css2?family=Lobster&display=swap');

    .navbar {
        background: #4B5270;
    }

    .navbar-brand {
        font-family: 'Lobster', cursive;
        font-size: 2.5rem;
    }

    .navbar-light > .navbar-brand {
        color: #20D3BD;
    }

    .navbar-expand {
        justify-content: center;
    }
</style>