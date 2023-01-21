<script setup>
    import {MoonIcon, SunIcon, UserCircleIcon} from '@heroicons/vue/24/outline'

    const props = defineProps(['dark'])
</script>

<template>

    <span class="text-gray-200 font-bold m-auto text-2xl">Name</span>
    <SunIcon v-if="dark" class="hover:cursor-pointer h-10 w-10 text-gray-100 my-2.5 hover:text-green-500 mr-10" @click="dark = !dark"/>
    <MoonIcon v-else class="hover:cursor-pointer h-10 w-10 text-gray-100 my-2.5 hover:text-green-500 mr-10" @click="dark = !dark"/>
    <UserCircleIcon class="text-gray-200 h-10 w-10 hover:text-green-500 mr-10 my-2.5"/>
</template>