<template>
  <div class="flex justify-center items-center h-screen bg-gray-200">
    <div class="justify-center items-center">
      <div class="md:flex flex-grow mx-auto">
        <div class="w-80 order-1 md:order-2">
          <img
            src="https://fastly.picsum.photos/id/94/800/500.jpg?hmac=hkU_p9BbJ9hhptwsU_FWsF2t7FIRXsl9XzJXL_si1M8"
            alt="Gambar Disamping Login"
            class="object-cover w-full h-full rounded"
          />
        </div>

        <div class="p-8 bg-white border rounded w-80 order-2 md:order-1">
          <h2 class="text-2xl font-semibold mb-4">Login CBT</h2>
          <div class="form-group mb-4">
            <label for="email" class="block text-sm font-medium">Email:</label>
            <input
              v-model="email"
              id="email"
              placeholder="Masukan email"
              class="w-full border p-2 rounded"
            />
          </div>
          <div class="form-group mb-4">
            <label for="password" class="block text-sm font-medium"
              >Password:</label
            >
            <div class="relative">
              <input
                :type="tampilkanPassword ? 'text' : 'password'"
                v-model="password"
                id="password"
                placeholder="********"
                class="w-full border p-2 rounded"
              />
              <input
                type="checkbox"
                v-model="tampilkanPassword"
                id="tampilkanPassword"
                class="absolute right-4 top-4"
              />
            </div>
          </div>

          <p class="text-center font-semibold">atau</p>

          <button
            @click="loginDenganGoogle"
            class="w-full my-4 bg-gray-300 py-2 rounded font-semibold"
          >
            Login dengan Google
          </button>

          <button
            @click="TombolLogin"
            class="w-full bg-blue-500 text-white py-2 rounded"
          >
            Login
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
const email = ref("");
const password = ref("");
const tampilkanPassword = ref(false);

const TombolLogin = () => {
  alert(`Email: ${email.value}\nPassword: ${password.value}`);
};

const loginDenganGoogle = () => {
  alert("Login dengan Google");
};
</script>
