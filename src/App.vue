<template>
  <div class="container">
    <PersonSearch
      class="search-area"
      @filterPersons="filterPersons"
    />
    <PersonsList
      class="list-area"
      :filtered_persons="filtered_persons"
      @selectPerson="selectPerson"
      @requestPersons="requestPersons"
    />
    <PersonDetails
      class="details-area"
      :selected_person="selected_person"
    />
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import PersonSearch from './components/PersonSearch.vue';
import PersonsList from './components/PersonsList.vue';
import PersonDetails from './components/PersonDetails.vue';
import { Person } from './person'

export default defineComponent({
  name: 'App',
  components: {
    PersonsList,
    PersonDetails,
    PersonSearch
  },
  data() {
    return {
      persons: [],
      filtered_persons: [],
      search_query: '',
      storage: '',
      selected_person: {}
    }
  },
  mounted() {
    const local_stored_persons = JSON.parse(localStorage.getItem('persons') || "null")
    if (local_stored_persons) {
      this.persons = local_stored_persons
    } else {
      this.requestPersons()
    }
    const local_stored_selected_person = JSON.parse(localStorage.getItem('selected_person') || "null")
    if (local_stored_selected_person) {
      this.selected_person = local_stored_selected_person
    }
  },
  methods: {
    requestPersons(requestAmount = 25) {
      fetch(`https://randomuser.me/api/?results=${requestAmount}`)
        .then(response => response.json())
        .then(data => {
          this.persons = this.persons.concat(data.results)
          this.filtered_persons = this.persons
          localStorage.setItem('persons', JSON.stringify(this.persons))
          this.filterPersons(this.search_query)
        })
    },
    filterPersons(search_query:string) {
      this.search_query = search_query
      this.filtered_persons = this.persons.filter((person:Person) => {
        return person.name.first.toLowerCase().includes(search_query.toLowerCase()) ||
               person.name.last.toLowerCase().includes(search_query.toLowerCase()) ||
               person.gender?.toLowerCase() == search_query.toLowerCase()
      })
    },
    selectPerson(index:number) {
      this.selected_person = this.filtered_persons[index]
      localStorage.setItem('selected_person', JSON.stringify(this.filtered_persons[index]))
    }
  }
});
</script>
<style scoped >
.container {
  display: grid;
  grid-template-columns: auto auto;
  grid-template-rows: auto;
  grid-template-areas: 
    "search ."
    "list details"
    "list details";
  grid-gap: 1rem;
  margin: 2rem auto;
  width: 70%;
}

.search-area {
  grid-area: search;
}

.list-area {
  grid-area: list;
}

.details-area {
  grid-area: details
}

@media screen and (max-width: 600px) {
  .container {
    grid-template-areas: 
      "details"
      "search"
      "list"
    ;
  }
}
</style>