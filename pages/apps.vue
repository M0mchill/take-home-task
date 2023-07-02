<template>
  <div class="d-flex flex-row">
    <v-card outlined shaped class="flex-grow-1 mr-2">
      <v-simple-table>
        <template v-slot:default>
          <thead>
            <tr>
              <th class="text-left">Title</th>
              <th class="text-left">Users</th>
            </tr>
          </thead>
          <tbody>
            <tr 
              v-for="app in apps" 
              :key="app.name" 
              @click="selectApp(app)" 
              :class="{ selected: selectedApp === app }">
              <td>{{ app.title }}</td>
              <td>{{ app.userCount }}</td>
            </tr>
          </tbody>
        </template>
      </v-simple-table>
    </v-card>

    <v-card outlined shaped class="flex-grow-1 mr-2">
      <div class="pa-2">App details</div>
      <div v-if="appDetails">Name: {{ appDetails.name }}</div>
      <!-- Display other app details as required -->
    </v-card>
  </div>
</template>

<script>
import { mapState } from 'vuex'
import axios from 'axios'

export default {
  data() {
    return {
      selectedApp: null,
      appDetails: null,
    }
  },

  mounted() {
    this.$store.dispatch('Apps/loadApps')
  },

  computed: {
    ...mapState('Apps', ['apps'])
  },

  methods: {
    async selectApp(app) {
      this.selectedApp = app
      try {
        const response = await axios.get(`/api/apps/${app.key}`)
        this.appDetails = response.data
      } catch (error) {
        console.error(error)
      }
    }
  }
}
</script>

<style scoped>
tbody tr {
  cursor: pointer;
}
tbody tr.selected {
  background-color: #ccc; 
}
</style>
