<script setup>

const props = defineProps([
    'dark'
])

</script>

<template>
    <button :class="`${dark ? 'bg-gray-600 text-green-500' : 'bg-gray-500 text-white'} mx-auto rounded-3xl hover:rounded-xl h-16 w-16 hover:bg-green-600 hover:text-white p-3 my-5`">
        <slot></slot>
    </button>
</template>
