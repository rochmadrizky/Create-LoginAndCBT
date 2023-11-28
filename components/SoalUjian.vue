<template>
  <div class="min-h-screen flex items-center justify-center">
    <div v-if="!mulaiSoal" class="bg-white p-8 rounded shadow-lg">
      <h1 class="text-3xl font-bold mb-4 text-center">Ujian Online</h1>
      <form @submit.prevent="kerjakanSoal">
        <div class="mb-4">
          <label for="nama" class="block text-sm font-medium text-gray-600">
            Nama:
          </label>
          <input
            v-model="nama"
            type="text"
            id="nama"
            class="border-2 rounded w-full px-3 py-2 mt-1 focus:outline-none focus:ring focus:border-blue-300"
            placeholder="Isi nama disini yaa..."
          />
        </div>

        <div class="mb-4">
          <label for="kelas" class="block text-sm font-medium text-gray-600">
            Kelas:
          </label>
          <input
            v-model="kelas"
            type="number"
            id="kelas"
            :min="1"
            :max="12"
            class="border-2 rounded w-full px-3 py-2 mt-1 focus:outline-none focus:ring focus:border-blue-300"
            placeholder="1"
          />
        </div>

        <div class="mb-4">
          <label class="block text-sm font-medium text-gray-600 my-2">
            Jenis Kelamin:
          </label>
          <div class="flex items-center">
            <input
              type="radio"
              id="laki-laki"
              v-model="jenisKelamin"
              value="laki-laki"
              class="mr-1"
            />
            <label for="laki-laki" class="text-sm">Laki-laki</label>
            <input
              type="radio"
              id="perempuan"
              v-model="jenisKelamin"
              value="perempuan"
              class="ml-4 mr-1"
            />
            <label for="perempuan" class="text-sm">Perempuan</label>
          </div>
        </div>

        <button
          type="submit"
          class="w-full bg-blue-500 text-white px-4 py-2 rounded focus:outline-none hover:bg-blue-600"
          :disabled="!inputValid"
        >
          Mulai
        </button>
      </form>
    </div>
    <!-- soal -->
    <div class="min-h-full flex items-center justify-center bg-gray-100">
      <div
        v-if="mulaiSoal"
        class="bg-white p-14 rounded-lg shadow-md w-full max-w-4xl"
      >
        <h1 class="text-3xl font-bold mb-4 text-center">Ujian Online</h1>

        <div class="flex">
          <div class="w-3/4 pr-6">
            <!-- Perubahan disini: Menambahkan padding kanan -->
            <div v-for="(question, key) in soalYangDilihat" :key="question.id">
              <p class="text-lg font-semibold mb-4">
                {{ question.id }}. {{ question.pertanyaan }}
              </p>
              <div
                v-for="answer in question.answers"
                :key="answer.content"
                class="flex items-center mb-2"
              >
                <input
                  type="radio"
                  :name="key"
                  :id="answer.content"
                  :value="answer.content"
                  v-model="question.jawabanYangDipilih"
                  :disabled="tampilkanHasil"
                  class="mr-2"
                />
                <label :for="answer.content" class="text-md">{{
                  answer.content
                }}</label>
              </div>
            </div>

            <div class="mt-4 flex justify-between">
              <button
                @click="kembaliPertanyaan"
                :disabled="indeksPertanyaan === 0"
                class="bg-blue-500 text-white px-4 py-2 rounded hover:bg-blue-600"
              >
                Sebelumnya
              </button>
              <button
                @click="nextPertanyaan"
                :disabled="indeksPertanyaan === soal.length - 1"
                class="bg-blue-500 text-white px-4 py-2 rounded hover:bg-blue-600"
              >
                Selanjutnya
              </button>
            </div>
          </div>

          <div class="w-1/4 flex items-center justify-center">
            <div class="relative">
              <div class="px-6 py-4 border flex items-center justify-center">
                <p class="text-4xl font-bold">
                  {{ Math.floor(timer / 60) }}:{{
                    timer % 60 < 10 ? "0" + (timer % 60) : timer % 60
                  }}
                </p>
              </div>
              <div
                class="absolute top-0 left-0 w-full h-full rounded-full transform rotate-90"
              ></div>
            </div>
          </div>
        </div>

        <button
          @click="kirimJawaban"
          class="bg-blue-500 text-white px-4 py-2 mt-4 rounded hover:bg-blue-600"
        >
          Submit
        </button>

        <div v-if="tampilkanHasil" class="mt-4">
          <h2 class="text-xl font-bold mb-2">Hasil Ujian</h2>
          <p>Nilai Anda: {{ skor }}</p>

          <div v-for="question in soal" :key="question.id" class="mb-4">
            <p
              :class="{
                'text-green-500':
                  question.jawabanYangDipilih === question.jawabanBenar,
                'text-red-500':
                  question.jawabanYangDipilih !== question.jawabanBenar,
              }"
            >
              Jawaban Anda: {{ question.jawabanYangDipilih }}
            </p>
            <p>Jawaban yang Benar: {{ question.jawabanBenar }}</p>
          </div>

          <p>Nama: {{ nama }}</p>
          <p>Kelas: {{ kelas }}</p>
          <p>Jenis Kelamin: {{ jenisKelamin }}</p>
          <p>
            Sisa Waktu: {{ Math.floor(timer / 60) }} menit
            {{ timer % 60 }} detik
          </p>
          <p>
            Waktu Pengerjaan: {{ Math.floor(waktuSelesai / 60) }} menit
            {{ waktuSelesai % 60 }} detik
          </p>

          <button
            @click="resetSemua"
            class="bg-red-500 text-white px-4 py-2 rounded hover:bg-red-600"
          >
            Keluar
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed, onMounted } from "vue";
import { soal } from "@/utils/soal";

const nama = ref("");
const kelas = ref("");
const jenisKelamin = ref("");
const mulaiSoal = ref(false);
const indeksPertanyaan = ref(0);
const timer = ref(300); // Timer 5 menit
const timerIntervalId = ref(null);
const waktuPengerjaan = ref(300); // Waktu pengerjaan awal
const waktuSelesai = ref(null);

onMounted(() => {
  // Bersihkan interval ketika komponen dipasang untuk memastikan tidak ada interval residual
  clearInterval(timerIntervalId.value);
});

const soalYangDilihat = computed(() => {
  return [soal[indeksPertanyaan.value]];
});

const nextPertanyaan = () => {
  if (indeksPertanyaan.value < soal.length - 1) {
    indeksPertanyaan.value++;
  }
};

const kembaliPertanyaan = () => {
  if (indeksPertanyaan.value > 0) {
    indeksPertanyaan.value--;
  }
};

const tampilkanHasil = ref(false);

const kirimJawaban = () => {
  // Hentikan interval timer
  clearInterval(timerIntervalId.value);

  // Tetapkan waktu selesai
  waktuSelesai.value = 300 - timer.value;

  tampilkanHasil.value = true;
  // ... (kode lain yang sudah ada)
};

const resetSemua = () => {
  mulaiSoal.value = false;
  tampilkanHasil.value = false;
  nama.value = "";
  kelas.value = "";
  jenisKelamin.value = "";
  soal.forEach((question) => {
    question.jawabanYangDipilih = null;
  });
  timer.value = 300; // Reset timer ke 5 menit
  waktuSelesai.value = null; // Reset waktu selesai
};

const skor = computed(() => {
  const jawabanBenar = soal.filter(
    (question) => question.jawabanYangDipilih === question.jawabanBenar
  ).length;

  const totalSoal = soal.length;

  return Math.floor((100 / totalSoal) * jawabanBenar);
});

const kerjakanSoal = () => {
  mulaiSoal.value = true;

  // Atur interval timer untuk berkurang setiap detik
  timerIntervalId.value = setInterval(() => {
    if (timer.value > 0) {
      timer.value--;
    } else {
      // Timer mencapai 0, secara otomatis kirim jawaban
      clearInterval(timerIntervalId.value);
      kirimJawaban();
    }
  }, 1000);
};

const inputValid = computed(() => {
  const namaValid = nama.value.trim() !== "";
  const isKelasValid =
    Number.isInteger(Number(kelas.value)) &&
    kelas.value >= 1 &&
    kelas.value <= 12;
  const isJenisKelaminValid = jenisKelamin.value !== "";
  return namaValid && isKelasValid && isJenisKelaminValid;
});
</script>
