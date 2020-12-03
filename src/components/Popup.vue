<template>
    <div @click="toggle" class="popup-post">
        <span>X</span>
        <h4>Id:{{ post.id }}</h4>
        <p>Title:{{ post.title }}</p>
        <p>Body:{{ post.body }}</p>
    </div>
</template>

<script>
export default {
    name: 'Popup',
    props: ['handleOpenPost', 'post'],
    data() {
        return {
            isClose: false,
        }
    },
    methods: {
        toggle() {
            this.handleOpenPost()
        },
    },
}
</script>

<style scoped>
.popup-post {
    position: relative;
    position: fixed;
    top: 50%;
    left: 50%;
    width: 80%;
    background: orange;
    color: #fff;
    padding: 10px;
    transform: translate(-50%, -50%);
    box-shadow: 0 0 20px #000;
    z-index: 99;
    display: block;
}

span {
    position: absolute;
    top: 10px;
    right: 10px;
    cursor: pointer;
}
</style>
