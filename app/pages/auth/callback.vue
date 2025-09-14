<script setup lang="ts">
const route = useRoute();
const router = useRouter();
const count = ref(5);
const timer = ref();

onMounted(() => {
    timer.value = setInterval(() => {
        count.value--;
        if (count.value <= 0) {
            clearInterval(timer.value);
            sessionStorage.setItem('token', String(route.query.token));
            sessionStorage.setItem('user_id', String(route.query.user_id));
            router.push('/chat');
        }
    }, 1000);
})
</script>

<template>
    <div class="bg-[#252728] text-white h-screen flex items-center justify-center flex-col gap-4">
        <h1 class="text-5xl font-bold">Đăng nhập thành công!</h1>
        <p class="text-lg">Tự động điều hướng sau {{ count }} giây</p>
    </div>
</template>