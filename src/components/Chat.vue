<script setup>
  import {PaperAirplaneIcon} from '@heroicons/vue/24/outline'
  import {ref, onMounted} from 'vue'
  import {supabase} from '../supabase'
  import AllAround from './AllAround.vue' 

  const channel = supabase
    .channel('table-db-changes')
    .on('postgres_changes', {
      event: 'INSERT',
      schema: 'public',
      table: 'messages'
    },
    (payload) => console.log(payload)
  ).subscribe();


  const message = ref("");
  const messages = ref([]);

  const allMessages = ref([])

  const getMessages = async() => {
    const {data} = await supabase.from('messages').select()

    allMessages.value = data
  }

  const sendMessage = async() => {
    const {data} = await supabase.from('messages').insert({id: allMessages.value.length+1, message: message.value})

    getMessages()

    message.value = ""
  }

  onMounted(getMessages)

</script>

<template>
  <AllAround />  

  
</template>

<style>
body{
  background-color: rgb(72, 77, 84);
}
</style>