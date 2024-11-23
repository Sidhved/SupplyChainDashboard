<!-- src/views/Dashboard.vue -->
<template>
  <div class="py-6">
    <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
      <!-- Stats Cards -->
      <div class="grid grid-cols-1 md:grid-cols-4 gap-6 mb-8">
        <div v-for="stat in stats" :key="stat.name" class="bg-white overflow-hidden shadow rounded-lg">
          <div class="p-5">
            <div class="flex items-center">
              <component :is="stat.icon" class="h-6 w-6" :class="stat.iconColor" />
              <div class="ml-5 w-0 flex-1">
                <dl>
                  <dt class="text-sm font-medium text-gray-500 truncate">{{ stat.name }}</dt>
                  <dd class="flex items-baseline">
                    <div class="text-2xl font-semibold text-gray-900">{{ stat.value }}</div>
                    <div :class="[stat.changeType === 'increase' ? 'text-green-600' : 'text-red-600', 'ml-2 flex items-center text-sm']">
                      {{ stat.change }}
                    </div>
                  </dd>
                </dl>
              </div>
            </div>
          </div>
        </div>
      </div>

      <!-- Charts Grid -->
      <div class="grid grid-cols-12 gap-6">
        <!-- Large Chart - Spans 8 columns -->
        <div class="col-span-12 lg:col-span-8 bg-white rounded-lg shadow p-6">
          <h3 class="text-lg font-medium text-gray-900 mb-4">Order Performance Trends</h3>
          <div class="h-[400px]">
            <LineChart :data="orderPerformanceData" :options="chartOptions" />
          </div>
        </div>

        <!-- Medium Chart - Spans 4 columns -->
        <div class="col-span-12 lg:col-span-4 bg-white rounded-lg shadow p-6">
          <h3 class="text-lg font-medium text-gray-900 mb-4">Inventory Distribution</h3>
          <div class="h-[400px]">
            <DoughnutChart :data="inventoryDistributionData" :options="doughnutOptions" />
          </div>
        </div>

        <!-- Medium Chart - Spans 6 columns -->
        <div class="col-span-12 lg:col-span-6 bg-white rounded-lg shadow p-6">
          <h3 class="text-lg font-medium text-gray-900 mb-4">Inventory Levels by Category</h3>
          <div class="h-[350px]">
            <BarChart :data="inventoryData" :options="chartOptions" />
          </div>
        </div>

        <!-- Medium Chart - Spans 6 columns -->
        <div class="col-span-12 lg:col-span-6 bg-white rounded-lg shadow p-6">
          <h3 class="text-lg font-medium text-gray-900 mb-4">Delivery Performance</h3>
          <div class="h-[350px]">
            <LineChart :data="deliveryPerformanceData" :options="chartOptions" />
          </div>
        </div>

        <!-- Full Width Chart - Spans all 12 columns -->
        <div class="col-span-12 bg-white rounded-lg shadow p-6">
          <h3 class="text-lg font-medium text-gray-900 mb-4">Supplier Performance Analysis</h3>
          <div class="h-[300px]">
            <BarChart :data="supplierPerformanceData" :options="horizontalBarOptions" />
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'
import { Line as LineChart, Bar as BarChart, Doughnut as DoughnutChart } from 'vue-chartjs'
import { 
  Chart as ChartJS, 
  Title, 
  Tooltip, 
  Legend, 
  LineElement, 
  LinearScale, 
  PointElement, 
  CategoryScale, 
  BarElement,
  ArcElement
} from 'chart.js'

ChartJS.register(
  CategoryScale, 
  LinearScale, 
  PointElement, 
  LineElement, 
  BarElement, 
  Title, 
  Tooltip, 
  Legend,
  ArcElement
)

const stats = ref([
  { name: 'Total Orders', value: '1,458', change: '+12.5%', changeType: 'increase', iconColor: 'text-blue-500' },
  { name: 'Deliveries', value: '1,387', change: '+8.2%', changeType: 'increase', iconColor: 'text-green-500' },
  { name: 'Returns', value: '71', change: '+2.1%', changeType: 'decrease', iconColor: 'text-red-500' },
  { name: 'Low Stock Items', value: '8', change: '-3', changeType: 'decrease', iconColor: 'text-yellow-500' }
])

const orderPerformanceData = {
  labels: ['Jan', 'Feb', 'Mar', 'Apr', 'May', 'Jun'],
  datasets: [
    {
      label: 'Orders',
      data: [1200, 1400, 1100, 1800, 1600, 2000],
      borderColor: '#2563eb',
      tension: 0.1
    },
    {
      label: 'Deliveries',
      data: [1150, 1380, 1050, 1750, 1580, 1950],
      borderColor: '#16a34a',
      tension: 0.1
    },
    {
      label: 'Returns',
      data: [50, 20, 50, 50, 20, 50],
      borderColor: '#dc2626',
      tension: 0.1
    }
  ]
}

const inventoryDistributionData = {
  labels: ['Electronics', 'Clothing', 'Food', 'Books', 'Toys'],
  datasets: [{
    data: [30, 25, 20, 15, 10],
    backgroundColor: [
      '#2563eb',
      '#16a34a',
      '#dc2626',
      '#ca8a04',
      '#9333ea'
    ]
  }]
}

const inventoryData = {
  labels: ['Electronics', 'Clothing', 'Food', 'Books'],
  datasets: [
    {
      label: 'Current Stock',
      data: [850, 1200, 450, 750],
      backgroundColor: '#2563eb'
    },
    {
      label: 'Minimum Required',
      data: [500, 800, 600, 400],
      backgroundColor: '#dc2626'
    }
  ]
}

const deliveryPerformanceData = {
  labels: ['Jan', 'Feb', 'Mar', 'Apr', 'May', 'Jun'],
  datasets: [
    {
      label: 'On-Time Delivery Rate',
      data: [95, 92, 96, 94, 97, 93],
      borderColor: '#16a34a',
      tension: 0.1
    },
    {
      label: 'Average Delivery Time (hours)',
      data: [24, 28, 22, 25, 20, 26],
      borderColor: '#2563eb',
      tension: 0.1
    }
  ]
}

const supplierPerformanceData = {
  labels: ['Supplier A', 'Supplier B', 'Supplier C', 'Supplier D', 'Supplier E'],
  datasets: [
    {
      label: 'On-Time Delivery Rate (%)',
      data: [98, 95, 92, 88, 85],
      backgroundColor: '#2563eb'
    },
    {
      label: 'Quality Rating',
      data: [95, 92, 88, 90, 86],
      backgroundColor: '#16a34a'
    }
  ]
}

const chartOptions = {
  responsive: true,
  maintainAspectRatio: false,
  plugins: {
    legend: {
      position: 'bottom'
    }
  }
}

const doughnutOptions = {
  responsive: true,
  maintainAspectRatio: false,
  plugins: {
    legend: {
      position: 'right'
    }
  }
}

const horizontalBarOptions = {
  responsive: true,
  maintainAspectRatio: false,
  indexAxis: 'y',
  plugins: {
    legend: {
      position: 'bottom'
    }
  }
}
</script>