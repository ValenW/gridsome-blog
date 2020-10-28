<template>
  <Layout>
    <!-- Page Header -->
    <header
      class="masthead"
      :style="{
        'background-image': `url(${GRIDSOME_API_URL}${general.cover.url})`,
      }"
    >
      <div class="overlay"></div>
      <div class="container">
        <div class="row">
          <div class="col-lg-8 col-md-10 mx-auto">
            <div class="site-heading">
              <h1>{{ general.title }}</h1>
              <span class="subheading">{{ general.subtitle }}</span>
            </div>
          </div>
        </div>
      </div>
    </header>

    <!-- Main Content -->
    <div class="container">
      <div class="row">
        <div class="col-lg-8 col-md-10 mx-auto">
          <div class="post-preview" v-for="{ node } in $page.posts.edges" :key="node.id">
            <g-link :to="`/post/${node.id}`">
              <h2 class="post-title">{{ node.title }}</h2>
              <!-- <h3 class="post-subtitle">Problems look mighty small from 150 miles up</h3> -->
            </g-link>
            <p class="post-meta">
              Posted by
              <a href="#">{{ `Valen Wang` }}</a>
              on {{ node.created_at }}
            </p>
            <p>
              <span v-for="tag in node.tags" :key="tag.id">
                <g-link :to="`/tag/${tag.id}`">{{ tag.title }}</g-link
                >&nbsp;
              </span>
            </p>
          </div>
          <hr />

          <!-- Pager -->
          <Pager :info="$page.posts.pageInfo" />
        </div>
      </div>
    </div>
  </Layout>
</template>

<page-query>
query($page: Int) {
  posts: allStrapiPost(perPage: 3, page: $page) @paginate {
    pageInfo {
      totalPages
      currentPage
    }
    edges {
      node {
        id
        title
        content
        cover {
          url
          height
          width
        }
        is_publish
        created_at
        tags {
          id
          title
        }
      }
    }
  }
  general: allStrapiGeneral {
    edges {
      node {
        id
        title
        subtitle
        cover {
          url
          width
          height
        }
      }
    }
  }
}
</page-query>

<script>
import { Pager } from "gridsome";

export default {
  name: "Index",
  components: {
    Pager,
  },
  computed: {
    general() {
      return this.$page.general.edges[0].node;
    },
  },
};
</script>

<style>
</style>
