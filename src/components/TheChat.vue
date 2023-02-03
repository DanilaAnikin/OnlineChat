<script setup>
import { ArrowLongRightIcon } from '@heroicons/vue/24/outline';
import {ref} from 'vue'
import { supabase } from '../supabase';

const props = defineProps(['allMessages', 'roomId'])

</script>

<template>
    <div class="max-w-6xl w-full mx-auto p-8 rounded-xl">
        <div v-if="roomId != undefined">
            <span v-for="message in allMessages" class="text-gray-300 flex max-w-7xl w-auto">
                <ArrowLongRightIcon class="w-4 h-4 mt-1 mr-3"/>
                <span class="break-all">{{ message.message }}</span>
            </span>
        </div>
    </div>
</template>
