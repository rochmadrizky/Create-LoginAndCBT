<template>
  <div class="flex justify-center items-center min-h-screen bg-gray-100 px-10">
    <div
      class="md:flex md:flex-grow max-w-screen-md mx-auto bg-white rounded-md overflow-hidden"
    >
      <div class="w-full">
        <img
          src="https://fastly.picsum.photos/id/94/800/500.jpg?hmac=hkU_p9BbJ9hhptwsU_FWsF2t7FIRXsl9XzJXL_si1M8"
          alt="Logo Sekolah"
          class="object-cover w-full h-full rounded"
        />
      </div>

      <div class="p-8 w-full">
        <div class="border-b border-gray-300 mb-6">
          <h1 class="text-center text-3xl font-semibold text-blue-500">
            Selamat datang,
          </h1>
          <p class="text-center py-2 text-gray-600">
            Lorem ipsum dolor sit amet consectetur adipisicing elit. Recusandae
            voluptates fugiat maiores culpa ab, dolores veritatis placeat
            blanditiis ad! Neque, veniam dolorem?
          </p>
        </div>

        <div class="mb-6">
          <h2 class="text-xl font-semibold mb-2">Ujian Hari Ini</h2>
          <ul class="list-disc pl-4">
            <li>{{ jamPertama }}</li>
            <li>{{ jamKedua }}</li>
          </ul>
          <p class="text-base mt-2">
            setiap satu mata ujian memiliki waktu 60 menit, gunakan waktu sebaik
            mungkin. semoga sukses dan lancar.
          </p>
        </div>

        <div class="flex items-center justify-center mt-4">
          <button
            class="px-8 py-2 bg-blue-500 text-white rounded hover:bg-blue-700 focus:outline-none focus:shadow-outline-blue"
          >
            Mulai Ujian
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
const jamPertama = ref("Matematika");
const jamKedua = ref("Inggris");
</script>
