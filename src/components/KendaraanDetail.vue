<template>
  <div
    v-if="data"
    class="bg-white rounded-xl shadow-lg w-full border border-gray-200 overflow-hidden transition-all duration-300"
  >
    <!-- Header -->
    <div class="bg-blue-600 text-white p-3 flex justify-between items-center">
      <h2 class="text-lg font-bold">Detail Pajak Kendaraan</h2>
      <button
        v-if="showCloseButton"
        class="text-white hover:bg-blue-700 rounded-full p-1 transition-colors duration-200 focus:outline-none"
        @click="$emit('close')"
      >
        <svg
          xmlns="http://www.w3.org/2000/svg"
          class="h-5 w-5"
          fill="none"
          viewBox="0 0 24 24"
          stroke="currentColor"
        >
          <path
            stroke-linecap="round"
            stroke-linejoin="round"
            stroke-width="2"
            d="M6 18L18 6M6 6l12 12"
          />
        </svg>
      </button>
    </div>

    <!-- Body -->
    <div class="p-4">
      

      <!-- Info Details -->
      <div class="space-y-3">
        
        <div class="flex border-b border-gray-100 pb-2">
          <span class="w-1/3 text-gray-500 text-sm">Nomor Plat</span>
          <span class="w-2/3 font-medium">{{ data.plat_nomor }}</span>
        </div>

        <div class="flex border-b border-gray-100 pb-2">
          <span class="w-1/3 text-gray-500 text-sm">Nama</span>
          <span class="w-2/3 font-medium">{{ data.nama }}</span>
        </div>

        <div class="flex border-b border-gray-100 pb-2">
          <span class="w-1/3 text-gray-500 text-sm">Alamat</span>
          <span class="w-2/3 font-medium">{{ data.alamat }}</span>
        </div>

        <div class="flex border-b border-gray-100 pb-2">
          <span class="w-1/3 text-gray-500 text-sm">Jenis Kendaraan</span>
          <span class="w-2/3 font-medium">{{ data.jenis_kendaraan }}</span>
        </div>

        <div class="flex border-b border-gray-100 pb-2">
          <span class="w-1/3 text-gray-500 text-sm">Nominal Pajak</span>
          <span class="w-2/3 font-medium"
            >Rp {{ formatRupiah(data.nominal_pajak) }}</span
          >
        </div>

        <div class="flex border-b border-gray-100 pb-2">
          <span class="w-1/3 text-gray-500 text-sm">Tanggal Pajak</span>
          <span class="w-2/3 font-medium">{{
            formatDate(data.tgl_pajak)
          }}</span>
        </div>

        <div class="flex pt-1">
          <span class="w-1/3 text-gray-500 text-sm">Status Pembayaran</span>
           <span
                class="inline-flex items-center rounded-full px-2.5 py-0.5 text-xs font-medium"
                :class="data.tgl_pembayaran 
                  ? 'bg-green-100 text-green-800' 
                  : 'bg-red-100 text-red-800'"
              >
                <span class="h-1.5 w-1.5 rounded-full mr-1.5" :class="data.tgl_pembayaran ? 'bg-green-600' : 'bg-red-600'"></span>
                {{ data.tgl_pembayaran ? 'Terbayar' : 'Belum Bayar' }}
              </span>
        </div>
      </div>
    </div>

    <!-- Footer -->
    <div
      class="bg-gray-50 p-3 flex justify-between items-center border-t border-gray-200"
    >
      

      <div>
        <button
          v-if="showActionButtons"
          class="px-3 py-1 bg-blue-600 text-white rounded-md hover:bg-blue-700 transition-colors duration-200 text-sm font-medium"
          @click="$emit('close')"
        >
          Kembali
        </button>
      </div>
    </div>
  </div>
</template>

<script setup>
import { computed } from "vue";

const props = defineProps({
  data: {
    type: Object,
    required: true,
  },
  showCloseButton: {
    type: Boolean,
    default: false,
  },
  showActionButtons: {
    type: Boolean,
    default: true,
  },
});

const lastUpdated = computed(() => {
  if (props.data.tgl_pembayaran) {
    return formatDate(props.data.tgl_pembayaran);
  }
  return formatDate(props.data.tgl_pajak);
});

function formatRupiah(num) {
  return num.toLocaleString("id-ID");
}

function formatDate(dateString) {
  if (!dateString) return "-";

  const options = { day: "numeric", month: "long", year: "numeric" };
  return new Date(dateString).toLocaleDateString("id-ID", options);
}
</script>
