<template>
    <div @click="getSinglePost(post.id)" class="post">
        <h4>Id:{{ post.id }}</h4>
        <p>Title:{{ post.title }}</p>
    </div>
</template>

<script>
export default {
    name: 'Post',
    props: ['post', 'handleOpenPost', 'getCurrentPost'],
    methods: {
        async getSinglePost(id) {
            const endpoint = 'https://graphqlzero.almansi.me/api'
            const query = `query {
                        post(id: ${id}) {
                            id
                            title
                            body
                        }
                    }`
            const response = await fetch(endpoint, {
                method: 'POST',
                headers: { 'content-type': 'application/json' },
                body: JSON.stringify({
                    query,
                }),
            })
            const data = await response.json()
            const post = data.data.post
            this.handleOpenPost()
            this.getCurrentPost(post)
        },
    },
}
</script>

<style scoped>
.post {
    background: rgb(218, 151, 27);
    color: #fff;
    padding: 5px;
    cursor: pointer;
}
</style>
