<template>
  <div class="mb-6">
    <!-- Title Section -->
    <div class="mb-4">
      <h2 class="text-lg font-bold text-gray-800">Ringkasan Pajak Kendaraan</h2>
      <p class="text-sm text-gray-500">Laporan total pembayaran dan tunggakan pajak kendaraan</p>
    </div>
    
    <!-- Cards Section -->
    <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
      <!-- Pajak Terbayar Card -->
      <div class="bg-white border border-green-200 rounded-xl shadow-sm overflow-hidden hover:shadow-md transition-shadow duration-300">
        <div class="p-4 flex items-start justify-between">
          <div>
            <p class="text-sm font-medium text-gray-500 mb-1">Pajak Terbayar</p>
            <div class="flex items-baseline">
              <span class="text-2xl font-bold text-gray-900">Rp {{ formatRupiah(terbayar) }}</span>
            </div>
            <div class="mt-1 flex items-center text-sm text-green-600">
              <svg xmlns="http://www.w3.org/2000/svg" class="h-4 w-4 mr-1" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 12l2 2 4-4m6 2a9 9 0 11-18 0 9 9 0 0118 0z" />
              </svg>
              <span>Terlunasi</span>
            </div>
          </div>
          
          <div class="rounded-full h-12 w-12 flex items-center justify-center bg-green-100">
            <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6 text-green-600" fill="none" viewBox="0 0 24 24" stroke="currentColor">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 8c-1.657 0-3 .895-3 2s1.343 2 3 2 3 .895 3 2-1.343 2-3 2m0-8c1.11 0 2.08.402 2.599 1M12 8V7m0 1v8m0 0v1m0-1c-1.11 0-2.08-.402-2.599-1M21 12a9 9 0 11-18 0 9 9 0 0118 0z" />
            </svg>
          </div>
        </div>
        
        <div class="bg-green-50 px-4 py-2 border-t border-green-100">
          <div class="flex justify-between items-center">
            <span class="text-xs text-gray-500">Status Pembayaran</span>
            <span class="inline-flex items-center rounded-full bg-green-100 px-2.5 py-0.5 text-xs font-medium text-green-800">
              <span class="h-1.5 w-1.5 rounded-full bg-green-600 mr-1"></span>
              Lunas
            </span>
          </div>
        </div>
      </div>
      
      <!-- Pajak Tunggakan Card -->
      <div class="bg-white border border-red-200 rounded-xl shadow-sm overflow-hidden hover:shadow-md transition-shadow duration-300">
        <div class="p-4 flex items-start justify-between">
          <div>
            <p class="text-sm font-medium text-gray-500 mb-1">Pajak Tunggakan</p>
            <div class="flex items-baseline">
              <span class="text-2xl font-bold text-gray-900">Rp {{ formatRupiah(tunggakan) }}</span>
            </div>
            <div class="mt-1 flex items-center text-sm text-red-600">
              <svg xmlns="http://www.w3.org/2000/svg" class="h-4 w-4 mr-1" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 8v4m0 4h.01M21 12a9 9 0 11-18 0 9 9 0 0118 0z" />
              </svg>
              <span>Belum Lunas</span>
            </div>
          </div>
          
          <div class="rounded-full h-12 w-12 flex items-center justify-center bg-red-100">
            <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6 text-red-600" fill="none" viewBox="0 0 24 24" stroke="currentColor">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M15 5v2m0 4v2m0 4v2M5 5a2 2 0 00-2 2v3a2 2 0 110 4v3a2 2 0 002 2h14a2 2 0 002-2v-3a2 2 0 110-4V7a2 2 0 00-2-2H5z" />
            </svg>
          </div>
        </div>
        
        <div class="bg-red-50 px-4 py-2 border-t border-red-100">
          <div class="flex justify-between items-center">
            <span class="text-xs text-gray-500">Status Pembayaran</span>
            <span class="inline-flex items-center rounded-full bg-red-100 px-2.5 py-0.5 text-xs font-medium text-red-800">
              <span class="h-1.5 w-1.5 rounded-full bg-red-600 mr-1"></span>
              Butuh Perhatian
            </span>
          </div>
        </div>
      </div>
    </div>
    
  </div>
</template>

<script setup>
import { computed } from 'vue'

const props = defineProps({
  terbayar: {
    type: Number,
    required: true
  },
  tunggakan: {
    type: Number,
    required: true
  }
})

const totalPajak = computed(() => {
  return props.terbayar + props.tunggakan
})


function formatRupiah(num) {
  if (!num && num !== 0) return "-";
  return Number(num).toLocaleString("id-ID");
}

</script>