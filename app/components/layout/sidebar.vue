<script setup lang="ts">
import api from '~/api/axios';

const chatboxes = ref([]);
const selectedMessageId = ref(0);

onMounted(() => {
    getChatRooms();
});

async function getChatRooms() {
    await api.get('/user/list-chat-room', {
        headers: {
            Authorization: "Bearer " + sessionStorage.getItem('token')
        }
    }).then((res) => {
        if (res.status == 200) {
            chatboxes.value = res.data.data;
        }
    }).catch((err) => {
        console.log(err);
    })
}

</script>

<template>
    <div class="flex flex-col gap-4 w-[26%] bg-[#252728] text-white select-none relative p-5 h-screen">
        <!-- Start header -->
        <div class="relative z-50 flex items-center justify-between">
            <h1 class="text-3xl font-bold">Đoạn chat</h1>
            <div class="flex items-center">
                <!-- Start header notifications menu -->
                <label for="header_notification" class="p-2 cursor-pointer group">
                    <Icon name="material-symbols:notifications-unread"
                        class="text-2xl transition-all duration-200 group-hover:brightness-75" />
                </label>
                <input type="checkbox" id="header_notification" hidden class="peer/header_notification">
                <div
                    class="gap-2 py-4 px-2 absolute right-0 hidden flex-col peer-checked/header_notification:flex rounded-xl outline-2 outline-[#383838] top-12 text-white bg-[#252728] w-80 h-[450px]">
                    <div class="flex justify-end">
                        <label for="header_notification"
                            class="px-1 text-xl text-white transition-all duration-200 cursor-pointer hover:opacity-75">
                            <Icon name="material-symbols:close-rounded" />
                        </label>
                    </div>
                    <ul
                        class="overflow-scroll overflow-x-hidden scrollbar scrollbar-thumb-[#3e3e3e] scrollbar-track-transparent">
                        <li v-for="i in 10" :key="i"
                            class="flex p-2.5 cursor-pointer gap-2 transition-all duration-200 hover:brightness-75">
                            <img src="../../assets/images/avatar.jpg" alt="avatar" class="h-12 rounded-full">
                            <h1>
                                <strong>Duong Dua</strong>
                                đã gửi lời mời kết bạn!
                            </h1>
                        </li>
                    </ul>
                </div>
                <!-- End header notifications menu -->

                <!-- Start header show more menu -->
                <label for="header_show_more"
                    class="bg-[#333334] p-2 rounded-full cursor-pointer transition-all duration-200 hover:brightness-90">
                    <Icon name="material-symbols:menu-rounded" class="text-2xl" />
                </label>
                <input type="checkbox" id="header_show_more" hidden class="peer/header_show_more" />
                <ul
                    class="p-2 absolute right-0 hidden flex-col peer-checked/header_show_more:flex rounded-xl outline-2 outline-[#383838] top-12 text-white bg-[#252728] w-52">
                    <li class="flex p-2.5 cursor-pointer gap-2 transition-all duration-200 hover:brightness-75">
                        <Icon name="material-symbols:info" class="text-2xl" />
                        <h1>Cập nhật thông tin</h1>
                    </li>
                    <li class="flex p-2.5 cursor-pointer gap-2 transition-all duration-200 hover:brightness-75">
                        <Icon name="material-symbols:edit-square" class="text-2xl" />
                        <h1>Soạn tin</h1>
                    </li>
                    <li class="flex p-2.5 cursor-pointer gap-2 transition-all duration-200 hover:brightness-75">
                        <Icon name="material-symbols:person-add-rounded" class="text-2xl" />
                        <h1>Kết bạn</h1>
                    </li>
                    <li class="flex gap-2 p-2.5 transition-all duration-200 cursor-pointer hover:brightness-75">
                        <Icon name="material-symbols:group-add" class="text-2xl" />
                        <h1>Tạo nhóm mới</h1>
                    </li>
                    <li
                        class="flex gap-2 p-2.5 pr-0 transition-all duration-200 cursor-pointer text-red-500 hover:brightness-75">
                        <Icon name="material-symbols:logout" class="text-2xl" />
                        <h1 class="font-medium">Đăng xuất</h1>
                    </li>
                </ul>
                <!-- End header show more menu -->
            </div>
        </div>
        <!-- End header -->

        <!-- Start search -->
        <div
            class="px-4 bg-[#333334] rounded-3xl justify-between flex items-center gap-3 outline-2 focus-within:outline-white hover:brightness-90 outline-transparent transition-all duration-200">
            <Icon name="material-symbols:search-rounded" class="text-2xl" />
            <div class="flex items-center w-full gap-2 border-[#7c7c7c]">
                <input type="text"
                    class="w-full py-3 text-white bg-transparent border-none outline-[#BC4D15] focus:outline-none"
                    placeholder="Tìm kiếm..." />
            </div>
        </div>
        <!-- End search -->

        <!-- Start classify -->
        <ul class="flex gap-1">
            <li>
                <input type="radio" id="classify_all" name="classify_radio" class="hidden peer/classify" checked />
                <label for="classify_all"
                    class="bg-transparent transition-all duration-200 flex items-center justify-center rounded-3xl min-w-16 hover:brightness-90 font-semibold hover:bg-[#3b3d3e] peer-checked/classify:bg-[#3b3d3e] py-1.5 px-4 cursor-pointer">
                    Tất cả
                </label>
            </li>
            <li>
                <input type="radio" id="classify_not_seen" name="classify_radio" class="hidden peer/classify" />
                <label for="classify_not_seen"
                    class="bg-transparent transition-all duration-200 flex items-center justify-center rounded-3xl min-w-16 hover:brightness-90 font-semibold hover:bg-[#3b3d3e] peer-checked/classify:bg-[#3b3d3e] py-1.5 px-4 cursor-pointer">
                    Chưa đọc
                </label>
            </li>
            <li>
                <input type="radio" id="classify_group" name="classify_radio" class="hidden peer/classify" />
                <label for="classify_group"
                    class="bg-transparent transition-all duration-200 flex items-center justify-center rounded-3xl min-w-16 hover:brightness-90 font-semibold hover:bg-[#3b3d3e] peer-checked/classify:bg-[#3b3d3e] py-1.5 px-4 cursor-pointer">
                    Nhóm
                </label>
            </li>
        </ul>
        <!-- End classify -->

        <!-- Start message -->
        <ul
            class="flex flex-col gap-1.5 overflow-scroll overflow-x-hidden scrollbar scrollbar-thumb-[#3e3e3e] scrollbar-track-transparent pr-2">
            <li v-for="(chatbox, index) in chatboxes" :key="chatbox.id" ref="items"
                @click="() => $router.push('/chat/' + chatbox.id)"
                :class="{ 'bg-[#3b3d3e]': selectedMessageId == chatbox.id }"
                class="flex items-center gap-3 p-3 rounded-xl cursor-pointer hover:brightness-90 transition-all duration-200 hover:bg-[#3b3d3e] group">
                <img class="rounded-full size-12" src="../../assets/images/avatar.jpg" alt="avatar" />
                <div class="flex items-center justify-between flex-grow">
                    <div class="w-44">
                        <h1 class="text-lg font-bold truncate">{{ chatbox.chatroom_name }}</h1>
                        <p class="text-sm font-medium text-white truncate">{{ chatbox.last_message.message }}</p>
                    </div>
                    <div class="flex items-center gap-2">
                        <div
                            class="shadow-sm shadow-black/50 flex items-center justify-center bg-[#333334] rounded-full p-1.5 opacity-0 group-hover:opacity-100 transition-all duration-200 hover:bg-[#4f5052]">
                            <Icon name="material-symbols:expand-more-rounded" class="text-2xl" />
                        </div>
                        <div class="flex flex-col items-end gap-2">
                            <p class="text-sm font-medium">1 giờ</p>
                            <div class="bg-green-600 rounded-full size-3" />
                        </div>
                    </div>
                </div>
            </li>
        </ul>
        <!-- End message -->
    </div>
</template>
