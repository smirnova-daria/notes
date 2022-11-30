<template>
  <main>
    <h1 class="text-4x1 font-bold mb-8">My Notes App</h1>

    <form @submit.prevent="registerOrLogin">
      <h3 class="text-x1 font-bold uppercase">Login or Register</h3>
      <label class="block mb-4">
        <input type="checkbox" v-model="isRegister" />
        Register?
      </label>
      <label class="block mb-4">
        <span class="block text-sm uppercase mb-2">Email</span>
        <input
          type="email"
          v-model="email"
          placeholder="Enter your email"
          class="block w-full text-slate-800 px-4 py-2"
        />
      </label>
      <label class="block mb-4">
        <span class="block text-sm uppercase mb-2">Password</span>
        <input
          type="password"
          v-model="password"
          placeholder="Enter your password"
          class="block w-full text-slate-800 px-4 py-2"
        />
      </label>

      <input
        type="submit"
        value="Login or Signup"
        class="text-green-500 hover:underline mb-4 cursor-pointer"
      />

      <!-- <p>
        Need to
        <button
          type="button"
          @click="() => (isRegister = !isRegister)"
          class="text-green-500 hover:underline"
        >
          {{ isRegister ? "Login" : "Register" }}</button
        >?
      </p> -->
    </form>
  </main>
</template>

<script setup>
import { ref } from "vue";
import { useRouter } from "vue-router";
import { useSignInEmailPassword, useSignUpEmailPassword } from "@nhost/vue";

const router = useRouter();

const isRegister = ref(false);

const email = ref("");
const password = ref("");
const { signUpEmailPassword } = useSignUpEmailPassword();
const { signInEmailPassword } = useSignInEmailPassword();

const registerOrLogin = async () => {
  if (!email.value || !password.value) {
    return alert("please fill in all fields");
  }

  const res = isRegister.value
    ? await signUpEmailPassword(email.value, password.value)
    : await signInEmailPassword(email.value, password.value);

  if (res.isError) return alert(res.error.message);

  router.push("/");
};
</script>

<style></style>
