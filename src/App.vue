<template>
    <div id="app">
        <button class="btn" @click="getPosts">Get all posts</button>
        <h1 v-if="isLoading">Loading...</h1>
        <div v-else class="posts">
            <Post
                v-for="(post, index) in getterOfShowedPosts"
                :key="index"
                :post="post"
                :handleOpenPost="openPost"
                :getCurrentPost="getCurrentPost"
            />
        </div>
        <button
            class="btn"
            @click="loadMore"
            v-if="showedPosts.length !== posts.length"
        >
            Load more...
        </button>

        <Popup v-if="isOpenPopup" :handleOpenPost="openPost" :post="post" />
    </div>
</template>

<script>
import Post from '@/components/Post'
import Popup from '@/components/Popup'
import ApolloClient, { gql } from 'apollo-boost'

export default {
    name: 'App',
    data() {
        return {
            isLoading: false,
            posts: [],
            showedPosts: [],
            noToShowPost: 5,
            post: {},
            isOpenPopup: false,
        }
    },
    components: {
        Post,
        Popup,
    },
    computed: {
        getterOfShowedPosts() {
            return this.posts.filter((post) => post.id <= this.noToShowPost)
        },
    },
    methods: {
        openPost() {
            this.isOpenPopup = !this.isOpenPopup
        },
        loadMore() {
            this.noToShowPost += 5
        },
        getCurrentPost(post) {
            this.post = post
        },

        async getPosts() {
            this.isOpenPopup = false
            this.isLoading = true
            const endpoint = 'https://graphqlzero.almansi.me/api'
            const query = `query (
              $options: PageQueryOptions
            ) {
              posts(options: $options) {
                data {
                  id
                  title
                }
                meta {
                  totalCount
                }
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
            this.noToShowPost = 5
            this.posts = response.data.posts.data
            this.isLoading = false
        },
    },
}
</script>

<style>
#app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
}

.btn {
    padding: 10px;
    background: orange;
    color: #fff;
    border: 0;
    box-shadow: 2px 2px 10px #000;
    cursor: pointer;
    margin: 0 auto;
    display: block;
}

.posts {
    margin: 10px 0;
    display: grid;
    grid-template-columns: 1fr;
    gap: 20px;
}

@media screen and (min-width: 568px) {
    .posts {
        display: grid;
        grid-template-columns: repeat(3, 1fr);
        gap: 20px;
    }
}
</style>
