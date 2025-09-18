<script setup lang="ts">
import type { AxiosProgressEvent } from 'axios';
import api from '~/api/axios';

const file = ref();
const progress = ref(0);

async function chooseFile(event: any) {
    file.value = event.target.files[0];
    let formData = new FormData();
    formData.append('file', file.value);
    await api.post('/upload', formData, {
        params: {
            'type': 'thumbnail'
        },
        onUploadProgress: (event: AxiosProgressEvent) => {
            if (event.total) {
                progress.value = Math.round((event.loaded * 100) / event.total);
            }
        }
    }).then((res) => {
        if (res.status == 200) {
            console.log(res.data);
        }
    }).catch((err) => {
        console.log(err);
    });
}
</script>

<template>
    <div class="mx-auto container h-screen flex items-center justify-center flex-col gap-5">
        <div>
            <input class="hidden" accept=".jpg,.jpeg,.png,.mp3" type="file" name="file" id="file"
                @change="chooseFile($event)">
            <label for="file" class="bg-gray-300 rounded-xl px-2 py-1.5 cursor-pointer hover:opacity-75">Choose
                file</label>
            <h1 class="font-bold text-5xl">Upload progress:
                <span v-if="progress < 100">{{ progress }}%</span>
                <span v-else>done!</span>
            </h1>
        </div>
        <div class="bg-gray-200 h-5 w-[1200px]">
            <div class="h-5 bg-green-500" :style="{ width: progress + '%' }"></div>
        </div>
    </div>
</template>