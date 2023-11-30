<template>
  <div
    class="flex justify-center items-center h-screen bg-cover"
    style="
      background-image: url('https://fastly.picsum.photos/id/957/800/500.jpg?hmac=9vdl26QSfV-rucdo-HeyyfzjpaKW4Ur1aR1oDtzbOAk');
    "
  >
    <div class="p-8 bg-white border rounded shadow-md w-96 mx-auto">
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
