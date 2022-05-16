<template>
  <input
    v-model="search_query"
    @input="handleSearch"
  />
</template>

<script lang="ts">
import { defineComponent } from "vue"

export default defineComponent({
  data() {
    return {
      search_query: ''
    }
  },
  mounted() {
    const local_stored_search_query = localStorage.getItem('search_query')
    if (local_stored_search_query) {
      this.search_query = local_stored_search_query
      this.$emit('filterPersons', this.search_query)
    }
  },
  methods: {
    handleSearch() {
      this.$emit('filterPersons', this.search_query)
      localStorage.setItem('search_query', this.search_query)
    }
  }
})
</script>
