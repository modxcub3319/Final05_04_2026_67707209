<template>
  <div class="container py-5">

    <div class="text-center mb-4">
      <h2 class="fw-bold text-primary">📋 Equipment Checkout Records</h2>
      <p class="text-muted">Data from Google Form via n8n Webhook</p>
    </div>

    <div class="card shadow-lg border-0 rounded-4">
      <div class="card-body">

        <div class="d-flex justify-content-between align-items-center mb-3">
          <h5 class="mb-0">Records</h5>
          <button class="btn btn-primary" @click="fetchData">
            🔄 Reload Data
          </button>
        </div>

        <div v-if="loading" class="text-center my-4">
          <div class="spinner-border text-primary"></div>
          <p class="mt-2">Loading data...</p>
        </div>

        <div class="table-responsive" v-if="records.length">
          <table class="table table-hover align-middle text-center">
            <thead class="table-primary">
              <tr>
                <th>#</th>
                <th>Timestamp</th>
                <th>Full Name</th>
                <th>Department</th>
                <th>Equipment Name</th>
                <th>Quantity</th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="(item, index) in records" :key="index">
                <td>{{ index + 1 }}</td>
                <td>{{ item.Timestamp }}</td>
                <td>{{ item['Full Name'] }}</td>
                <td>{{ item.Department }}</td>
                <td>{{ item['Equipment Name'] }}</td>
                <td>{{ item.Quantity }}</td>
              </tr>
            </tbody>
          </table>
        </div>

        <div v-else-if="!loading" class="text-center text-muted py-4">
          ❌ No data available
        </div>

      </div>
    </div>

  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'

const records = ref([])
const loading = ref(false)

const fetchData = async () => {
  loading.value = true
  try {
    const response = await fetch('http://localhost:5678/webhook/data3')
    const data = await response.json()
    records.value = data
  } catch (error) {
    console.error('Error:', error)
  }
  loading.value = false
}

onMounted(() => {
  fetchData()
})
</script>

<style>
body {
  background-color: #f8f9fa;
}
.table td {
  white-space: nowrap;
}
</style>