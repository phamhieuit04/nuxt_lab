<template>
    <div class="h-screen w-96 bg-green-500 p-5 text-white">
        <h1 class="text-5xl font-bold">Đây là sidebar</h1>
        <NuxtLink to="/" class="block py-5 text-2xl underline">Home</NuxtLink>
        <ul class="flex flex-col gap-5">
            <NuxtLink
                v-for="i in 10"
                :to="`/posts/${i}`"
                class="text-2xl underline"
                >Post {{ i }}
            </NuxtLink>
        </ul>
    </div>
</template>
