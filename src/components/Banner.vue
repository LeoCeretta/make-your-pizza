<template>
    <div id="main-banner">
        <h1>Make Your Pizza</h1>
    </div>
</template>

<script>export default {
    name: "Banner"
}
</script>

<style scoped>
    #main-banner {
        background-image: url('/img/pizza.jpg');
        background-size: cover;
        height: 550px;
        display: flex;
        align-items: center;
        justify-content: flex-start;
    }

    #main-banner h1 {
        color: #fff;
        text-align: center;
        font-size: 70px;
        margin-left: 200px;
        margin-top: 50px;
    }

</style>