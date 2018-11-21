<template>
  <div id="app" class="container my-4">
    <h1>vue-plain-pagination</h1>
    <p class="lead text-muted">Vue Component</p>
    <p>Element for showing pagination to indicate a series of related content exists across multiple pages. Component
      code based on article "<a href="https://alligator.io/vuejs/vue-pagination-component/" target="_blank">Build a
        Reusable Pagination Component with Vue.js</a>".</p>
    <p class="text-success">Current page: <strong>{{ currentPage }}</strong></p>
    <nav class="mb-4">
      <v-pagination v-model="currentPage"
                    :page-count="30"
                    :classes="bootstrapPaginationClasses"
                    :labels="customLabels"></v-pagination>
    </nav>
    <hr>
    <p class="text-muted small">
      View on
      <a href="https://github.com/cichy380/vue-pagination" class="text-muted" target="_blank">GitHub</a>
    </p>
  </div>
</template>

<script>
import vPagination from './components/vue-plain-pagination'

export default {
  name: 'app',
  data() {
    return {
      currentPage: 1,
      bootstrapPaginationClasses: { // http://getbootstrap.com/docs/4.1/components/pagination/
        ul: 'pagination',
        li: 'page-item',
        liActive: 'active',
        liDisable: 'disabled',
        button: 'page-link'
      },
      customLabels: {
        first: 'First',
        prev: 'Previous',
        next: 'Next',
        last: 'Last'
      }
    }
  },
  components: {
    vPagination
  }
}
</script>
