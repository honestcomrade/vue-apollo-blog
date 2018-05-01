<template>
  <h2 v-if="loading > 0">
    Loading...
  </h2>
  <div v-else>
    <article>
      <h1>{{post.title}}</h1>
      <commit-chart></commit-chart>
      <vue-markdown>{{post.content}}</vue-markdown>
    </article>
  </div>
</template>

<script>
  // chart component
  import CommitChart from './chart-components/CommitChart';
  // markdown support
  import VueMarkdown from 'vue-markdown';
  import gql from 'graphql-tag';

  const post = gql`
    query post($slug: String!) {
      post: Post(slug: $slug) {
        id
        slug
        title
        # coverImage {
        #   handle
        # }
        content
        dateAndTime
      }
    }
  `

  export default {
    name: 'PostPage',
    data: () => ({
      loading: 0
    }),
    apollo: {
      $loadingKey: 'loading',
      post: {
        query: post,
        variables () {
          return {
            slug: this.$route.params.slug
          }
        }
      }
    },
    components: { VueMarkdown, CommitChart }
  }
</script>

<style scoped>
  .placeholder {
    height: 366px;
    background-color: #eee;
  }
</style>
