<template>
    <div @click="getSinglePost(post.id)" class="post">
        <h4>Id:{{ post.id }}</h4>
        <p><strong>Title:</strong>{{ post.title }}</p>
    </div>
</template>

<script>
import ApolloClient, { gql } from 'apollo-boost'
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
            const client = new ApolloClient({
                uri: endpoint,
            })
            const response = await client.query({
                query: gql`
                    ${query}
                `,
            })

            const post = response.data.post
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
