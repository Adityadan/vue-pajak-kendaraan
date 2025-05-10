<template>
  <div class="overflow-hidden rounded-lg border border-gray-200 shadow-sm">
    <!-- Table -->
    <div class="overflow-x-auto">
      <table class="w-full min-w-full divide-y divide-gray-200">
        <!-- Header -->
        <thead class="bg-gray-50">
          <tr>
            <th scope="col" class="px-4 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider w-16 text-center">No</th>
            <th scope="col" class="px-4 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">Nomor Plat</th>
            <th scope="col" class="px-4 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">Jenis Kendaraan</th>
            <th scope="col" class="px-4 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">Tanggal Pajak</th>
            <th scope="col" class="px-4 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">Status</th>
            <th scope="col" class="px-4 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider w-32 text-center">Aksi</th>
          </tr>
        </thead>
        
        <!-- Body -->
        <tbody class="bg-white divide-y divide-gray-200">
          <tr 
            v-for="(item, index) in data" 
            :key="item.plat_nomor"
            class="hover:bg-gray-50 transition-colors duration-150"
          >
            <td class="px-4 py-3 whitespace-nowrap text-sm text-gray-500 text-center">{{ index + 1 }}</td>
            
            <td class="px-4 py-3 whitespace-nowrap">
              <span class="font-medium text-gray-900">{{ item.plat_nomor }}</span>
            </td>
            
            <td class="px-4 py-3 whitespace-nowrap">
              <span class="text-sm text-gray-700">{{ item.jenis_kendaraan }}</span>
            </td>
            
            <td class="px-4 py-3 whitespace-nowrap">
              <span class="text-sm text-gray-700">{{ formatDate(item.tgl_pajak) }}</span>
            </td>
            
            <td class="px-4 py-3 whitespace-nowrap">
              <span
                class="inline-flex items-center rounded-full px-2.5 py-0.5 text-xs font-medium"
                :class="item.tgl_pembayaran 
                  ? 'bg-green-100 text-green-800' 
                  : 'bg-red-100 text-red-800'"
              >
                <span class="h-1.5 w-1.5 rounded-full mr-1.5" :class="item.tgl_pembayaran ? 'bg-green-600' : 'bg-red-600'"></span>
                {{ item.tgl_pembayaran ? 'Terbayar' : 'Belum Bayar' }}
              </span>
            </td>
            
            <td class="px-4 py-3 whitespace-nowrap text-center">
              <button
                class="inline-flex items-center justify-center rounded-md bg-blue-50 px-3 py-1.5 text-sm font-medium text-blue-700 hover:bg-blue-100 transition-colors focus:outline-none focus:ring-2 focus:ring-blue-500 focus:ring-offset-2"
                @click="$emit('detail', item)"
              >
                <svg xmlns="http://www.w3.org/2000/svg" class="h-4 w-4 mr-1" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                  <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M15 12a3 3 0 11-6 0 3 3 0 016 0z" />
                  <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M2.458 12C3.732 7.943 7.523 5 12 5c4.478 0 8.268 2.943 9.542 7-1.274 4.057-5.064 7-9.542 7-4.477 0-8.268-2.943-9.542-7z" />
                </svg>
                Detail
              </button>
            </td>
          </tr>
          
          <!-- Empty state -->
          <tr v-if="data.length === 0">
            <td colspan="6" class="px-4 py-8 text-center text-gray-500">
              <div class="flex flex-col items-center justify-center">
                <svg xmlns="http://www.w3.org/2000/svg" class="h-12 w-12 text-gray-300 mb-2" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                  <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M20 13V6a2 2 0 00-2-2H6a2 2 0 00-2 2v7m16 0v5a2 2 0 01-2 2H6a2 2 0 01-2-2v-5m16 0h-2.586a1 1 0 00-.707.293l-2.414 2.414a1 1 0 01-.707.293h-3.172a1 1 0 01-.707-.293l-2.414-2.414A1 1 0 006.586 13H4" />
                </svg>
                <p class="text-sm">Tidak ada data yang tersedia</p>
              </div>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
    
    <!-- Footer -->
    <div class="bg-gray-50 border-t border-gray-200 px-4 py-2 flex items-center justify-between">
      <div class="text-sm text-gray-500 flex items-center">
        <svg xmlns="http://www.w3.org/2000/svg" class="h-4 w-4 mr-1 text-gray-400" fill="none" viewBox="0 0 24 24" stroke="currentColor">
          <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 12h6m-6 4h6m2 5H7a2 2 0 01-2-2V5a2 2 0 012-2h5.586a1 1 0 01.707.293l5.414 5.414a1 1 0 01.293.707V19a2 2 0 01-2 2z" />
        </svg>
        Total Data: <span class="font-medium ml-1">{{ data.length }}</span>
      </div>
    </div>
  </div>
</template>

<script setup>
defineProps(["data"]);

function formatDate(dateString) {
  if (!dateString) return '-';
  
  const options = { day: 'numeric', month: 'long', year: 'numeric' };
  return new Date(dateString).toLocaleDateString('id-ID', options);
}
</script>