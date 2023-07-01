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
            <tr v-for="app in apps" :key="app.name" @click="selectApp(app)">
              <td>{{ app.title }}</td>
              <td>{{ app.userCount }}</td>
            </tr>
          </tbody>
        </template>
      </v-simple-table>
    </v-card>

    <v-card outlined shaped class="flex-grow-1 mr-2">
      <div class="pa-2">App details</div>
    </v-card>
  </div>
</template>

<script>
import { mapState } from 'vuex'

export default {
  mounted() {
    this.$store.dispatch('Apps/loadApps')
  },

  computed: {
    ...mapState('Apps', ['apps'])
  },

  methods: {
    selectApp(app) {
      // TODO: Show this app in the box on the right
    }
  }
}
</script>

<style scoped>
tbody tr {
  cursor: pointer;
}
</style>
