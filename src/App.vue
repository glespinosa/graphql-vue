<template>
    <div id="app">
        <button class="btn" @click="getPosts">Get all posts</button>
        <h1 v-if="isLoading">Loading...</h1>
        <div v-else class="posts">
            <Post
                v-for="(post, index) in posts.data"
                :key="index"
                :post="post"
                :handleOpenPost="openPost"
                :getCurrentPost="getCurrentPost"
            />
        </div>
        <Popup v-if="isOpenPopup" :handleOpenPost="openPost" :post="post" />
    </div>
</template>

<script>
import Post from '@/components/Post'
import Popup from '@/components/Popup'

export default {
    name: 'App',
    data() {
        return {
            isLoading: false,
            posts: [],
            post: {},
            isOpenPopup: false,
        }
    },
    components: {
        Post,
        Popup,
    },
    methods: {
        openPost() {
            this.isOpenPopup = !this.isOpenPopup
        },
        getCurrentPost(post) {
            this.post = post
        },

        async getPosts() {
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
            this.isLoading = true
            const response = await fetch(endpoint, {
                method: 'POST',
                headers: { 'content-type': 'application/json' },
                body: JSON.stringify({
                    query,
                }),
            })
            const data = await response.json()
            this.posts = data.data.posts
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
