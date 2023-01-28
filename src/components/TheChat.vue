<script setup>
import { ArrowLongRightIcon } from '@heroicons/vue/24/outline';
import {ref} from 'vue'
import { supabase } from '../supabase';

const props = defineProps(['allMessages', 'roomId'])

</script>

<template>
    <div class="max-w-6xl w-full mx-auto p-8 rounded-xl">
        <input v-if="addPop && roomId != null" v-model="inputAdd" class="bg-transparent border border-green-600 rounded-lg p-2 ml-4 placeholder-slate-400 text-slate-300 focus:text-white" placeholder="His username...">
        <button v-if="addPop && roomId != null" class="bg-gray-900 text-slate-200 py-2 px-4 ml-4 rounded-xl" @click="addUser">Add</button>
        <div v-if="roomId != undefined">
            <span v-for="message in allMessages" class="text-gray-300 flex max-w-7xl w-auto">
                <ArrowLongRightIcon class="w-4 h-4 mt-1 mr-3"/>
                <span class="break-all">{{ message.message }}</span>
            </span>
        </div>
    </div>
</template>

<style scoped>
    .vyjebanina{
        height: 48rem;
    }
</style>