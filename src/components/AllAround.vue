<script setup>
import {SunIcon, MoonIcon, UserCircleIcon, FireIcon, HashtagIcon, BanknotesIcon, FingerPrintIcon, Cog8ToothIcon} from '@heroicons/vue/24/outline'
import {ref} from 'vue'
import ButtonComp from './Button.vue'
import Channels from './Channels.vue'
import TopBar from './TopBar.vue'

const dark = ref(true)

const arr = ref([FireIcon, HashtagIcon, BanknotesIcon, FingerPrintIcon, Cog8ToothIcon])
</script>

<template>
    <div class="flex">

        <div :class="`${dark ? 'bg-gray-800 text-gray-200' : 'bg-gray-700 text-gray-900'} w-32 p-5 h-screen`">
            <ButtonComp v-for="icon in arr" :dark="dark">
                <component :is="icon"/>
            </ButtonComp>
        </div>

        <div :class="`flex ${dark ? 'bg-gray-700' : 'bg-gray-600'} text-gray-200 w-72 p-10 h-screen`">
            <Channels/>
        </div>
        <div :class="`${dark ? 'bg-gray-800' : 'bg-gray-700'} flex w-screen h-16 justify-between`">
            <TopBar :dark="dark"/>
        </div>
    </div>
</template>