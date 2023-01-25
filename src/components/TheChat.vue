<script setup>
import { ArrowLongRightIcon } from '@heroicons/vue/24/outline';

const props = defineProps(['allMessages'])

</script>

<template>
    <div class="max-w-6xl w-full mx-auto p-8 rounded-xl flex-col h-96 mt-96">
        <span v-for="message in allMessages" class="text-gray-300 flex rounded-xl max-w-3xl w-full">
            <ArrowLongRightIcon class="w-4 h-4 mt-1 mr-3"/>
            <span class="break-all">{{ message.message }}</span>
        </span>
    </div>
</template>
