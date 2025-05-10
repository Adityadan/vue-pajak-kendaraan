<template>
  <div class="bg-white rounded-lg shadow-sm border border-gray-200 p-4 mb-6">
    <div class="flex items-center justify-between mb-3">
      <h3 class="font-medium text-gray-700">Filter Data</h3>
      
      <!-- Reset Button -->
      <button 
        @click="resetFilters" 
        class="text-sm text-gray-500 hover:text-blue-600 flex items-center transition-colors"
        :class="{ 'opacity-50 cursor-not-allowed': !hasActiveFilters }"
        :disabled="!hasActiveFilters"
      >
        <svg xmlns="http://www.w3.org/2000/svg" class="h-4 w-4 mr-1" fill="none" viewBox="0 0 24 24" stroke="currentColor">
          <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 4v5h.582m15.356 2A8.001 8.001 0 004.582 9m0 0H9m11 11v-5h-.581m0 0a8.003 8.003 0 01-15.357-2m15.357 2H15" />
        </svg>
        Reset
      </button>
    </div>
    
    <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
      <!-- Jenis Kendaraan Filter -->
      <div>
        <label for="jenis" class="block text-sm font-medium text-gray-700 mb-1">Jenis Kendaraan</label>
        <div class="relative">
          <select 
            id="jenis"
            v-model="jenis" 
            class="block w-full rounded-md border-gray-300 py-2 pl-3 pr-10 text-base focus:border-blue-500 focus:outline-none focus:ring-blue-500 sm:text-sm appearance-none border"
            @change="emitFilter"
          >
            <option value="">Semua Jenis</option>
            <option value="mobil">Mobil</option>
            <option value="sepeda motor">Sepeda Motor</option>
          </select>
          <div class="pointer-events-none absolute inset-y-0 right-0 flex items-center px-2 text-gray-700">
            <svg class="h-4 w-4" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 20 20" fill="currentColor" aria-hidden="true">
              <path fill-rule="evenodd" d="M5.293 7.293a1 1 0 011.414 0L10 10.586l3.293-3.293a1 1 0 111.414 1.414l-4 4a1 1 0 01-1.414 0l-4-4a1 1 0 010-1.414z" clip-rule="evenodd" />
            </svg>
          </div>
        </div>
        <p v-if="jenis" class="mt-1 text-xs text-blue-600">
          Menampilkan: {{ jenisLabel }}
        </p>
      </div>
      
      <!-- Status Pembayaran Filter -->
      <div>
        <label for="status" class="block text-sm font-medium text-gray-700 mb-1">Status Pembayaran</label>
        <div class="relative">
          <select 
            id="status"
            v-model="status" 
            class="block w-full rounded-md border-gray-300 py-2 pl-3 pr-10 text-base focus:border-blue-500 focus:outline-none focus:ring-blue-500 sm:text-sm appearance-none border"
            @change="emitFilter"
          >
            <option value="">Semua Status</option>
            <option value="terbayar">Terbayar</option>
            <option value="belum_bayar">Belum Bayar</option>
          </select>
          <div class="pointer-events-none absolute inset-y-0 right-0 flex items-center px-2 text-gray-700">
            <svg class="h-4 w-4" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 20 20" fill="currentColor" aria-hidden="true">
              <path fill-rule="evenodd" d="M5.293 7.293a1 1 0 011.414 0L10 10.586l3.293-3.293a1 1 0 111.414 1.414l-4 4a1 1 0 01-1.414 0l-4-4a1 1 0 010-1.414z" clip-rule="evenodd" />
            </svg>
          </div>
        </div>
        <p v-if="status" class="mt-1 text-xs text-blue-600">
          Menampilkan: {{ statusLabel }}
        </p>
      </div>
    </div>
    
    <!-- Active Filters Display -->
    <div v-if="hasActiveFilters" class="mt-4 pt-3 border-t border-gray-200">
      <div class="flex flex-wrap gap-2">
        <span class="text-xs text-gray-500">Filter aktif:</span>
        
        <div v-if="jenis" class="inline-flex items-center rounded-full bg-blue-50 px-2 py-1 text-xs font-medium text-blue-700">
          {{ jenisLabel }}
          <button class="ml-1 text-blue-500 hover:text-blue-700" @click="clearJenis">
            <svg xmlns="http://www.w3.org/2000/svg" class="h-3 w-3" viewBox="0 0 20 20" fill="currentColor">
              <path fill-rule="evenodd" d="M4.293 4.293a1 1 0 011.414 0L10 8.586l4.293-4.293a1 1 0 111.414 1.414L11.414 10l4.293 4.293a1 1 0 01-1.414 1.414L10 11.414l-4.293 4.293a1 1 0 01-1.414-1.414L8.586 10 4.293 5.707a1 1 0 010-1.414z" clip-rule="evenodd" />
            </svg>
          </button>
        </div>
        
        <div v-if="status" class="inline-flex items-center rounded-full bg-blue-50 px-2 py-1 text-xs font-medium text-blue-700">
          {{ statusLabel }}
          <button class="ml-1 text-blue-500 hover:text-blue-700" @click="clearStatus">
            <svg xmlns="http://www.w3.org/2000/svg" class="h-3 w-3" viewBox="0 0 20 20" fill="currentColor">
              <path fill-rule="evenodd" d="M4.293 4.293a1 1 0 011.414 0L10 8.586l4.293-4.293a1 1 0 111.414 1.414L11.414 10l4.293 4.293a1 1 0 01-1.414 1.414L10 11.414l-4.293 4.293a1 1 0 01-1.414-1.414L8.586 10 4.293 5.707a1 1 0 010-1.414z" clip-rule="evenodd" />
            </svg>
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'

const emit = defineEmits(['filter'])

const jenis = ref('')
const status = ref('')

const jenisLabel = computed(() => {
  switch(jenis.value) {
    case 'mobil': return 'Mobil'
    case 'sepeda motor': return 'Sepeda Motor'
    default: return ''
  }
})

const statusLabel = computed(() => {
  switch(status.value) {
    case 'terbayar': return 'Terbayar'
    case 'belum_bayar': return 'Belum Bayar'
    default: return ''
  }
})

const hasActiveFilters = computed(() => {
  return jenis.value !== '' || status.value !== ''
})

function emitFilter() {
  emit('filter', { jenis: jenis.value, status: status.value })
}

function resetFilters() {
  jenis.value = ''
  status.value = ''
  emitFilter()
}

function clearJenis() {
  jenis.value = ''
  emitFilter()
}

function clearStatus() {
  status.value = ''
  emitFilter()
}
</script>