<template>
  <div
    v-show="selected_person?.name?.first"
    class="details card"
  >
    <div class="details-header">
      <img
        class="details-photo"
        :src="selected_person?.picture?.large"
        :alt="selected_person?.name?.last"
      >
      <p class="title">{{ selected_person?.name?.first }} {{ selected_person?.name?.last }}</p>
      <p v-show="selected_person?.id?.name">
        <label>ID: {{ selected_person?.id?.name }} {{ selected_person?.id?.value }}</label>
      </p>
      <p>
        Cellphone: {{ selected_person?.cell }}
      </p>
      <p>
        Phone: {{ selected_person?.phone }}
      </p>
      <p>
        Email: {{ selected_person?.email }}
      </p>
      <div>
        Address:
        <p>
          {{ selected_person?.location?.street.name }} 
          {{ selected_person?.location?.street.number }}
        </p>
        <p>
          {{ selected_person?.location?.postcode }} 
          {{ selected_person?.location?.state }} 
        </p>
        <p>
          {{ selected_person?.location?.country }}
        </p>
      </div>
    </div>
  </div>
</template>
<script lang="ts">
import { defineComponent } from 'vue';
import { Person } from '../person'

export default defineComponent({
  props: {
    selected_person: {
      type: Object as () => Person
    }
  }
})
</script>
<style>
.card {
  box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2);
  width: auto;
  margin: auto;
  text-align: center;
  padding: .5rem;
}

.details-photo {
  border-radius: 50%;
}

.title {
  color: grey;
  font-size: 18px;
}
</style>