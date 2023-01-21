

<script setup>
    import {ref} from 'vue'
    import { ChevronDoubleRightIcon } from '@heroicons/vue/24/outline';

    const channels = ref(['tvoje mama', 'picu voe', 'jeste jednou', 'prestaaaan'])
</script>


<template>
    <div class="mt-5">
        <span class="text-xl font-bold h-10">Channels</span>
        <div class="mt-16">
            <span v-for="channel in channels" class="flex my-5">
                <ChevronDoubleRightIcon class="h-5 w-5 mr-3"/>
                {{channel}}
            </span>
        </div>
    </div>
</template>