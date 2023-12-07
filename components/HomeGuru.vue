<template>
  <div class="flex justify-center items-center min-h-screen bg-gray-100">
    <div
      class="md:flex md:flex-grow max-w-screen-md mx-auto bg-white rounded-md shadow-lg overflow-hidden"
    >
      <div class="p-8 w-full">
        <div class="border-b border-gray-300 mb-6">
          <h1 class="text-center text-3xl font-semibold text-blue-500">
            Selamat datang, Guru!
          </h1>
          <p class="text-center py-2 text-gray-600">
            Lorem ipsum dolor sit amet consectetur adipisicing elit. Dolorem,
            repudiandae.
          </p>
        </div>

        <div class="mb-6">
          <h2 class="text-xl font-semibold mb-2">Tambahkan Soal Ujian</h2>
          <form @submit.prevent="tambahkanSoal">
            <div class="mb-4">
              <label
                for="mataPelajaran"
                class="block text-gray-700 text-sm font-semibold mb-2"
              >
                Mata Pelajaran
              </label>
              <select
                v-model="mataPelajaran"
                id="mataPelajaran"
                name="mataPelajaran"
                class="w-full px-3 py-2 border rounded-md"
              >
                <option value="bahasaIndonesia">Bahasa Indonesia</option>
                <option value="matematika">Matematika</option>
              </select>
            </div>

            <div class="mb-4">
              <label
                for="soal"
                class="block text-gray-700 text-sm font-semibold mb-2"
              >
                Soal
              </label>
              <textarea
                v-model="soal"
                id="soal"
                name="soal"
                rows="4"
                class="w-full px-3 py-2 border rounded-md"
              ></textarea>
            </div>

            <div
              v-for="(jawaban, index) in jawabanArray"
              :key="index"
              class="mb-4"
            >
              <label
                :for="`jawaban${index}`"
                class="block text-gray-700 text-sm font-semibold mb-2"
              >
                Jawaban {{ String.fromCharCode(65 + index) }}
              </label>
              <input
                v-model="jawabanArray[index]"
                :id="`jawaban${index}`"
                :name="`jawaban${index}`"
                class="w-full px-3 py-2 border rounded-md"
              />
            </div>

            <div class="mb-4">
              <label
                for="jawabanBenar"
                class="block text-gray-700 text-sm font-semibold mb-2"
              >
                Jawaban Benar (Misal: A)
              </label>
              <input
                v-model="jawabanBenar"
                type="text"
                id="jawabanBenar"
                name="jawabanBenar"
                class="w-full px-3 py-2 border rounded-md"
              />
            </div>

            <div class="flex items-center justify-center">
              <button
                type="submit"
                class="px-8 py-2 bg-blue-500 text-white rounded hover:bg-blue-700 focus:outline-none focus:shadow-outline-blue"
              >
                Tambahkan Soal
              </button>
            </div>
          </form>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref } from "vue";

const mataPelajaran = ref("");
const soal = ref("");
const jawabanArray = ref(["", "", "", ""]);
const jawabanBenar = ref("");

const tambahkanSoal = () => {
  console.log("Mata Pelajaran:", mataPelajaran.value);
  console.log("Soal:", soal.value);
  console.log("Jawaban:", jawabanArray.value);
  console.log("Jawaban Benar:", jawabanBenar.value);
};
</script>
