<script setup lang="ts">
import api from '~/api/axios';

async function signIn() {
    await api.get('/auth/redirect').then((res) => {
        if (res.status == 200) {
            window.location.assign(res.data.data.url);
        }
    }).catch((err) => {
        console.log(err);
    });
}
</script>

<template>
    <div class="flex h-screen items-center justify-center bg-[#252728] select-none">
        <div class="flex w-96 flex-col gap-4 rounded-xl bg-[#252728] px-4 py-4 text-white outline-2 outline-[#383838]">
            <div class="flex flex-col gap-6">
                <div class="flex flex-col items-center gap-2">
                    <img src="../../assets/images/logo.webp" alt="logo" class="h-32" />
                    <h1 class="text-3xl font-bold">Xin chào!</h1>
                </div>
                <div class="flex flex-col gap-2">
                    <button @click="signIn()"
                        class="flex cursor-pointer gap-4 rounded-2xl border border-gray-300 px-5 py-4 transition-all duration-200 hover:border-transparent hover:bg-green-500">
                        <img class="w-8" src="../../assets/images/google.png" alt="google icon" />
                        <p class="text-xl">Tiếp tục với Google</p>
                    </button>
                </div>
            </div>
        </div>
    </div>
</template>
