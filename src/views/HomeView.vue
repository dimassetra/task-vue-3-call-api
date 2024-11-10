<template>
  <v-container>
    <v-row>
      <!-- Account List Section -->
      <v-col cols="12">
        <v-card>
          <v-card-title class="text-center">
            <span class="headline">Account List</span>
          </v-card-title>
          <v-card-text>
            <v-row>
              <v-col
                v-for="list in lists"
                :key="list.list_id"
                cols="12"
                md="6"
                lg="4"
              >
                <v-card class="mb-4" outlined>
                  <v-card-title>{{ list.name }}</v-card-title>
                  <v-card-subtitle>Balance: {{ list.balance }}</v-card-subtitle>
                </v-card>
              </v-col>
            </v-row>
          </v-card-text>
        </v-card>
      </v-col>

      <!-- Account Statistics Section -->
      <v-col cols="12">
        <v-card class="mt-4">
          <v-card-title class="text-center">
            <span class="headline">Account Statistics</span>
          </v-card-title>
          <v-card-text>
            <v-row>
              <v-col
                v-for="statistics in stats"
                :key="statistics.stats_id"
                cols="12"
                md="4"
              >
                <v-card class="mb-4" outlined>
                  <v-card-title>Total Users: {{ statistics.total_user }}</v-card-title>
                  <v-card-subtitle>Total Balance: {{ statistics.total_balance }}</v-card-subtitle>
                  <v-card-subtitle>Average Balance: {{ statistics.average_balance }}</v-card-subtitle>
                </v-card>
              </v-col>
            </v-row>
          </v-card-text>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>

<script setup>
import { ref, onMounted } from 'vue'
import axios from 'axios'
import { useUserStore } from '@/stores/user'

const stats = ref([]) // Array to store account data
const lists = ref([])
const userStore = useUserStore()

// Function to fetch account statistics data from the backend
const fetchStats = async () => {
  try {
    const response = await axios.get('http://localhost:8080/account/statistics', {
      headers: {
        Authorization: userStore.token // Include the token in the request headers
      }
    })

    // Assign the account statistics data directly from response.data
    stats.value = response.data
  } catch (error) {
    console.error('Error fetching account statistics:', error)
  }
}

// Function to fetch account list from the backend
const fetchList = async () => {
  try {
    const response = await axios.get('http://localhost:8080/account/list')

    // Assign the account list data directly from response.data
    lists.value = response.data.data
  } catch (error) {
    console.error('Error fetching account list:', error)
  }
}

// Fetch data on component mount
onMounted(() => {
  fetchStats()
  fetchList()
})
</script>

<style scoped>
/* Styling the titles */
.headline {
  font-weight: bold;
  font-size: 24px;
  color: #1976D2;
}

/* Spacing for account cards */
.v-card {
  padding: 20px;
  border-radius: 8px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
}

/* Adding borders to card titles and subtitles */
.v-card-title {
  font-size: 18px;
  font-weight: 600;
}

.v-card-subtitle {
  font-size: 16px;
  color: #555;
}

/* Adding space between the cards */
.v-row {
  margin-top: 20px;
}

.mb-4 {
  margin-bottom: 16px;
}
</style>
