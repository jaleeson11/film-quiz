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
    .navbar-brand {
        font-size: 2.5rem;
        font-weight: 700;
    }

    .navbar-light > .navbar-brand {
        color: #fff;
    }

    .navbar-expand {
        justify-content: center;
    }
</style>