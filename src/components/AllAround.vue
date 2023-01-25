<script setup>
import {SunIcon, MoonIcon, UserCircleIcon, FireIcon, HashtagIcon, BanknotesIcon, FingerPrintIcon, Cog8ToothIcon, PaperAirplaneIcon} from '@heroicons/vue/24/outline'
import {onMounted, ref} from 'vue'
import {supabase} from '../supabase'
import ButtonComp from './Button.vue'
import Channels from './Channels.vue'
import TopBar from './TopBar.vue'
import TheChat from './TheChat.vue'
import Login from './Login.vue'
import SignUp from './SignUp.vue'


const dark = ref(true)
const arr = ref([FireIcon, HashtagIcon, BanknotesIcon, FingerPrintIcon, Cog8ToothIcon])
const message = ref("")
const allMessages = ref([])
const roomId = ref()
const nameOfGroup = ref("")
const logged = ref(false)
const activeUser = ref()
const signUpped = ref(true)

const channel = supabase
    .channel('table-db-changes')
    .on('postgres_changes', {
    event: 'INSERT',
    schema: 'public',
    table: 'messages',
    filter: `room=eq.${roomId.value}`
    },
    (payload) => allMessages.value.push(payload.new),

).subscribe();

const login = (userId) => {
    logged.value = true
    activeUser.value = userId
}

const signup = () => {
    signUpped.value = true
    logged.value = false
}

const goSignup = () => {
    signUpped.value = false
    logged.value = true
}

const goLogin = () => {
    signUpped.value = true
    logged.value = false
}

const openRoom = (id) => {
    roomId.value = id
    getMessages()
}

const getName = async() => {
    if(roomId.value != undefined){
        const {data} = await supabase.from('rooms').select().eq('id', roomId.value)
        const blabla = data
        nameOfGroup.value = blabla[0].name
    }
}

const getMessages = async() => {
    const {data} = await supabase.from('messages').select().eq('room', roomId.value)

    allMessages.value = data
    getName()
}

const sendMessage = async() => {

    if(message.value !== ""){
        const {data} = await supabase.from('messages').insert({'message': message.value, 'room': roomId.value, 'user': activeUser.value})
        message.value = ""
        getMessages()
    }
}

onMounted(getMessages)

</script>

<template>
    <div v-if="!logged">
        <Login @login="login" @goSignup="goSignup"/>
    </div>
    <div v-if="!signUpped">
        <SignUp @signup="signup" @goLogin="goLogin"/>
    </div>

    <div v-if="logged && signUpped" class="flex">

        <div :class="`${dark ? 'bg-gray-800 text-gray-200' : 'bg-gray-700 text-gray-900'} w-32 p-5 h-screen`">

            <ButtonComp v-for="icon in arr" :dark="dark">
                <component :is="icon"/>
            </ButtonComp>
        
        </div>

        <div :class="`flex ${dark ? 'bg-gray-700' : 'bg-gray-600'} text-gray-200 w-72 p-10 h-screen`">
            <Channels @openRoom="openRoom" :activeUser="activeUser"/>
        </div>

        <div class="w-screen">
            
            <div :class="`${dark ? 'bg-gray-800' : 'bg-gray-700'} justify-between flex h-16 shadow-lg shadow-gray-900`">
                <TopBar :dark="dark" :name="nameOfGroup"/>
            </div>
            
            <div class="flex-col justify-end">
                <TheChat class="justify-end overflow-y-scroll" :allMessages="allMessages"/>
                
                <div class="mt-44 ml-52 max-w-6xl w-full flex">
                    <input v-model="message" class="max-w-6xl w-full text-gray-200 p-3 border flex-col justify-end border-other-100 focus:text-black focus:bg-other-300 rounded-lg focus:ring-gray-800 bg-transparent focus:placeholder-gray-800 placeholder-gray-100" placeholder="Chat with other people..."/>
                    <PaperAirplaneIcon class="h-7 w-7 text-gray-200 inline-block mt-2.5 -ml-10 hover:cursor-pointer hover:text-green-500" @click="sendMessage"/>
                </div>
            </div>
        
        </div>
    </div>
</template>

<style>
    body{
        background-color: rgb(72, 77, 84);
    }
</style>

