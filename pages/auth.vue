<script setup lang="ts">
import { v4 as uuid } from 'uuid'

useSeoMeta({
  title: 'Auth | CRM System',
});

const emailRef = ref('');
const passwordRef = ref('');
const nameRef = ref('');

const isLoadingStore = useIsLoadingStore();
const authStore = useAuthStore();
const router = useRouter();

const login = async () => {
  isLoadingStore.set(true);
  try{
    await account.createEmailSession(emailRef.value, passwordRef.value);
    const response = await account.get();

    if (response) {
      authStore.set({
        email: response.email,
        name: response.name,
        status: response.status,
      });
    }

    emailRef.value = '';
    passwordRef.value = '';
    nameRef.value = '';

    await router.push('/');
  } catch (e){
    alert(e);
  } finally {
    isLoadingStore.set(false);
  }
};

const register = async () => {
  try{
    await account.create(
      uuid(),
      emailRef.value,
      passwordRef.value,
      nameRef.value
    );
    await login();
  } catch (e){
    alert(e);
  }
};
</script>

<template>
  <div class="flex items-center justify-center min-h-screen w-full">
    <div class="rounded bg-sidebar w-1/4 p-5">
      <h1 class="text-2xl font-bold text-center mb-5">Auth</h1>

      <form>
        <UiInput
          v-model="emailRef"
          placeholder="Email"
          type="email"
          class="mb-3"
        />
        <UiInput
          v-model="passwordRef"
          placeholder="Password"
          type="password"
          class="mb-3"
        />
        <UiInput
          v-model="nameRef"
          placeholder="Name"
          type="name"
          class="mb-3"
        />
        <div class="flex items-center justify-center gap-5">
          <UiButton type="button" @click="login">Login</UiButton>
          <UiButton type="button" @click="register">Register</UiButton>
        </div>
      </form>
    </div>
  </div>
</template>
