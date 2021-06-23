<template>
    <Layout>

        <!-- Learn how to use images here: https://gridsome.org/docs/images -->
        <g-image alt="Example image" src="~/favicon.png" width="135" />

        <p>
    <h1>Posts</h1>
    <ul>
        <li v-for="{ node } in $static.posts.edges" :key="node.id">
            {{ node.title }}
        </li>
    </ul>


    <h2>Search</h2>
    <input type="text" name="search" id="search" placeholder="Type something..." v-model="search">

    <div v-if="search.length > 0 && searchResults.length > 0">
        <article v-for="post in searchResults" :key="post.node.id">
            <h1><g-link :to="post.node.path">{{ post.node.title }}</g-link></h1>
            <p>{{ post.node.excerpt }}</p>
            <p>{{ post.node.date }}</p>
        </article>
    </div>

    <div v-else>
        <p>Your search didn't return any results. Please try again.</p>
    </div>
</p>

    </Layout>
</template>

<static-query>
{
    posts(first:1000) {
        edges {
            node {
                id
                slug
                title
                content
                excerpt
                date
            }
        }
    }
}
</static-query>

<script>
export default {
    metaInfo: {
        title: 'Areal Colectiv'
    },
    data() {
        return {
            search: ''
        }
    },
    computed: {
        searchResults() {
            return this.$static.posts.edges.filter(post => {
                let search_string = this.search.toLowerCase().trim();
                return post.node.title.toLowerCase().includes(search_string) || post.node.content.toLowerCase().includes(search_string)
            })
        }
    }
}
</script>

<style>
.home-links a {
    margin-right: 1rem;
}
</style>
