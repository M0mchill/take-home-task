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
      <div v-if="appDetails">Description: {{ appDetails.description }}</div>
      <div v-if="appDetails">
        <p :class='usersClass'>
          Users: {{ appDetails.totalUsers }} out of {{appDetails.userLimit}}
        </p>
      </div>
      <div v-if="appDetails">
        <p :class='usageClass'>
        Usage: {{ appDetails.usage }} out of {{ appDetails.usageLimit }}
        </p>
      </div> 
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
    ...mapState('Apps', ['apps']),
    
    usageClass() {
      if (this.appDetails && this.appDetails.usageLimit !== 0) {
        let usageRatio = this.appDetails.usage/this.appDetails.usageLimit*100;
        if (usageRatio > 75) {
          return 'red';
        } else if (usageRatio > 50) {
          return 'yellow';
        } else {
          return 'green';
        }
      } else {
        return '';
      }
    },

    usersClass() {
      if (this.appDetails && this.appDetails.userLimit !== 0) {
        let usersRatio =  this.appDetails.totalUsers/this.appDetails.userLimit*100 ;
        if (usersRatio >= 75) {
          return 'red';
        } else if (usersRatio >= 50) {
          return 'yellow';
        } else {
          return 'green';
        }
      } else {
        return '';
      }
    }
  },


  methods: {
    async selectApp(app) {
      this.selectedApp = app
      try {
        const response = await fetch(`http://localhost:5000/api/apps/${app.key}`,{method: 'GET'})
        this.appDetails =  await response.json()
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
  background-color: #ccc; /* Choose your own color for selected row */
}
p .red {
  color:red
}
p .yellow {
  color:yellow
}
p .green {
  color:green
}
</style>
