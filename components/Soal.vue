<template>
  <div class="min-h-screen">
    <div v-if="!mulaiSoal" class="text-center mt-8">
      <button
        @click="
          mulaiSoal = true;
          startTimer();
        "
        class="w-full bg-blue-500 text-white px-4 py-2 rounded focus:outline-none hover:bg-blue-600"
      >
        Mulai Ujian
      </button>
    </div>

    <!-- soal -->
    <div v-if="mulaiSoal" class="bg-white p-14 rounded-lg shadow-md">
      <h1 class="text-3xl font-bold mb-4 text-center">Ujian Online</h1>

      <div class="flex flex-wrap items-center justify-center">
        <div
          class="flex items-center justify-center md:order-2 md:w-1/2 my-2 mx-2"
        >
          <div class="w-24 md:w-48">
            <div class="right-0 grid grid-cols-3">
              <div v-for="(question, key) in soal" :key="question.id">
                <div
                  class="border rounded p-2 md:p-3 text-center cursor-pointer"
                  @click="gotoSoal(key)"
                  :class="{
                    'bg-green-500': question.jawabanYangDipilih,
                    'bg-gray-400 cursor-not-allowed':
                      key !== indeksPertanyaan && !question.jawabanYangDipilih,
                  }"
                >
                  {{ question.id }}
                </div>
              </div>
            </div>

            <div class="my-2">
              <div class="border">
                <div class="flex items-center justify-center px-4 py-1">
                  <p class="text-4xl font-bold text-center">
                    {{ Math.floor(timer / 60) }}:{{
                      timer % 60 < 10 ? "0" + (timer % 60) : timer % 60
                    }}
                  </p>
                </div>
              </div>
            </div>
          </div>
        </div>

        <div
          class="flex items-center justify-center md:order-1 md:w-1/2 my-2 mx-2"
        >
          <div>
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
                <label :for="answer.content" class="text-md">
                  {{ answer.content }}
                </label>
              </div>
            </div>

            <!-- tombol next, prev dan submit -->
            <div>
              <div class="mt-4 flex gap-2">
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

              <button
                @click="kirimJawaban"
                class="bg-blue-500 text-white px-4 py-2 mt-4 rounded hover:bg-blue-600"
              >
                Submit
              </button>
            </div>
          </div>
        </div>
      </div>
    </div>

    <!-- hasil ujian -->
    <div v-if="tampilkanHasil" class="flex items-center justify-center my-5">
      <div>
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

        <button
          @click="resetSemua"
          class="bg-red-500 text-white px-4 py-2 rounded hover:bg-red-600"
        >
          Keluar
        </button>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed, onMounted } from "vue";
import { soal } from "@/utils/soal";

const mulaiSoal = ref(false);
const indeksPertanyaan = ref(0);
const timerIntervalId = ref(null);
const waktuPengerjaan = 3600; // Waktu pengerjaan awal 1 jam
const timer = ref(waktuPengerjaan); // Timer sesuai kebutuhan
const waktuSelesai = ref(null);

const gotoSoal = (key) => {
  if (key >= 0 && key < soal.length) {
    indeksPertanyaan.value = key;
  }
};

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
  waktuSelesai.value = waktuPengerjaan - timer.value;

  tampilkanHasil.value = true;
  // ... (kode lain yang sudah ada)
};

const resetSemua = () => {
  mulaiSoal.value = false;
  tampilkanHasil.value = false;
  soal.forEach((question) => {
    question.jawabanYangDipilih = null;
  });
  timer.value = waktuPengerjaan; // Reset timer ke waktu pengerjaan awal
  waktuSelesai.value = null; // Reset waktu selesai
};

const skor = computed(() => {
  const jawabanBenar = soal.filter(
    (question) => question.jawabanYangDipilih === question.jawabanBenar
  ).length;

  const totalSoal = soal.length;

  return Math.floor((100 / totalSoal) * jawabanBenar);
});

const startTimer = () => {
  // Atur interval timer untuk berkurang setiap detik
  timerIntervalId.value = setInterval(() => {
    if (timer.value > 0) {
      timer.value--;
    } else {
      // Timer mencapai 0, secara otomatis kirim jawaban
      clearInterval(timerIntervalId.value);
      kirimJawaban();
    }

    // Periksa apakah ada soal yang belum dijawab dan waktu habis
    if (
      timer.value === 0 &&
      soal.some((question) => question.jawabanYangDipilih === null)
    ) {
      soal.forEach((question) => {
        // Jika jawaban belum diisi, isi dengan nilai default atau yang diinginkan
        if (question.jawabanYangDipilih === null) {
          question.jawabanYangDipilih = "Tidak diisi"; // Atau sesuai kebutuhan
        }
      });

      // Kirim jawaban secara otomatis
      kirimJawaban();
    }
  }, 1000);
};
</script>

<style scoped>
/* Tambahkan gaya CSS jika diperlukan */
</style>
