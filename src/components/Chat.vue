<script setup>
  import {PaperAirplaneIcon} from '@heroicons/vue/24/outline'
  import {ref, onMounted} from 'vue'
  import {supabase} from '../supabase'


  const channel = supabase.channel('room');
  const message = ref("");
  const messages = ref([]);

  const allMessages = ref([])
  
  channel.on('broadcast', {event: 'message'}, (payload) => messages.value.push(payload.payload));

  channel.subscribe((status) => {
    if(status === 'SUBSCRIBED'){
      console.log('connected')
    }
  })

  const sendMessage = async() => {
    channel.send({
      type: 'broadcast',
      event: 'message',
      payload: message.value
    })

    const {sendData} = await supabase.from('messages').insert({id: allMessages.value.length+1, message: message.value})

    message.value = "";
    getMessages()
  }

  const getMessages = async() => {
    const {data} = await supabase.from('messages').select()

    allMessages.value = data;
  }

  onMounted(getMessages)

</script>

<template>

  <div class="m-auto max-w-lg w-full p-8 bg-slate-700 text-slate-200 rounded-2xl my-20">
      <p class="text-2xl font-bold">Chat:</p>
      <div class="h-60 max-w-sm w-full bg-slate-500 p-6 text-xl rounded-xl mx-auto mt-10">
          <span v-for="message in allMessages">
            {{ message.message }}
            <br>
          </span>
      </div>
      <div class="flex justify-center mt-10">
          <input v-model="message" class="ml-10 mr-5 rounded-xl resize text-slate-100 focus:text-slate-700 bg-slate-500 focus:ring-0 focus:border-transparent border:transparent p-4 focus:bg-slate-200 focus:placeholder:text-slate-600 placeholder:text-slate-100" placeholder="Chat with other people"/>
          <button class="mr-10 mt-2 bg-slate-300 h-10 w-10 p-2 rounded-lg text-slate-600" @click="sendMessage">
              <PaperAirplaneIcon class="mx-auto h-5 w-5 text-slate-600"/>
          </button>
      </div>
  </div>

</template>