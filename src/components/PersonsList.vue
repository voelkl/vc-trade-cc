<template>
  <ul class="list">
    <li
      v-for="(person, index) in filtered_persons"
      :key="'person'+ index"
      class="list-item"
      @click="$emit('selectPerson', index)"
    >
      <img class="thumbnail" :src="person.picture?.thumbnail" />
      <span>{{ person.name.first }} {{ person.name.last }}</span>
    </li>
    <a
      @click="$emit('requestPersons')"
      href="#"
      ref="moreResults"
    >
      More Results
    </a>
  </ul>
</template>
<script lang="ts">
import { defineComponent } from 'vue';
import { Person } from '../person'

export default defineComponent({
  props: {
    filtered_persons: {
      type: Array as () => Array<Person>
    },
    active: {
      type: Number
    }
  },
  mounted() {
    this.intersectionObserver()
  },
  methods: {
    intersectionObserver() {
      const observer = new IntersectionObserver((entries) => {
        entries.forEach(entry => {
          if (entry.isIntersecting) {
            return this.$emit('requestPersons')
          }
        })
      })
      let ref: Element = this.$refs.moreResults as Element
      observer.observe(ref)
    }
  }
})
</script>
<style scoped >
.list {
  max-height: 30rem;
  overflow-y: scroll;
  padding: 0;
}

.list-item {
  align-items: center;
  display: flex;
  cursor: pointer;
  margin: 1rem 0;
  text-transform: capitalize;
}

.thumbnail {
  border-radius: 50%;
  margin-right: 1rem;
}
</style>