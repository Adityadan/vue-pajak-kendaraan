<template>
  <div class="p-6 max-w-6xl mx-auto">
    <!-- Rekap Pajak -->
    <RekapCard :terbayar="rekap.terbayar" :tunggakan="rekap.tunggakan" />

    <!-- Filter -->
    <FilterForm v-if="!selectedItem" @close="selectedItem" @filter="applyFilter" />

    <!-- Jika sedang lihat detail kendaraan -->
    <KendaraanDetail
      v-if="selectedItem"
      :data="selectedItem"
      @close="selectedItem = null"
    />

    <!-- Jika tidak sedang lihat detail, tampilkan tabel -->
    <KendaraanTable v-else :data="kendaraanList" @detail="showDetail" />

    <!-- Loading -->
    <div v-if="loading" class="text-center mt-4 text-gray-500">
      Memuat data...
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted, watch } from "vue";
import axios from "axios";

import RekapCard from "./components/RekapCard.vue";
import FilterForm from "./components/FilterForm.vue";
import KendaraanTable from "./components/KendaraanTable.vue";
import KendaraanDetail from "./components/KendaraanDetail.vue";

const kendaraanList = ref([]);

const rekap = ref({ terbayar: 0, tunggakan: 0 });

const selectedItem = ref(null);

const loading = ref(false);

const filters = ref({
  jenis_kendaraan: "",
  status_pembayaran: "",
});

const fetchRekap = async () => {
  try {
    const res = await axios.get(
      "http://127.0.0.1:8000/api/pajak/kendaraan/rekap"
    );
    rekap.value = res.data.data;
  } catch (err) {
    console.error("Gagal ambil data rekap:", err);
  }
};

const fetchKendaraan = async () => {
  loading.value = true;
  try {
    const query = new URLSearchParams();

    if (filters.value.jenis_kendaraan) {
      query.append("jenis_kendaraan", filters.value.jenis_kendaraan);
    }
    if (filters.value.status_pembayaran) {
      query.append("status_pembayaran", filters.value.status_pembayaran);
    }

    const res = await axios.get(
      `http://127.0.0.1:8000/api/pajak/kendaraan?${query.toString()}`
    );
    kendaraanList.value = res.data.data;
  } catch (err) {
    console.error("Gagal ambil data kendaraan:", err);
  } finally {
    loading.value = false;
  }
};

const applyFilter = (newFilters) => {
  filters.value = {
    jenis_kendaraan: newFilters.jenis,
    status_pembayaran: newFilters.status,
  };
};

const showDetail = (item) => {
  console.log(item);
  
  selectedItem.value = item;
};

// Load awal
onMounted(() => {
  fetchRekap();
  fetchKendaraan();
});

// Refresh list saat filter berubah
watch(filters, fetchKendaraan);
</script>
