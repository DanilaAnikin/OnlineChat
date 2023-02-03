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
const addPop = ref(false)
const inputAdd = ref('')


const changeTheme = () => {
    if(!dark.value){
        document.body.classList.remove('dark');
    } else  {
        document.body.classList.add('dark');
    }
    dark.value = !dark.value
}

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
    const channel = supabase
        .channel('table-db-changes')
        .on(
            'postgres_changes',
            {
            event: 'INSERT',
            schema: 'public',
            table: 'messages',
            filter: `room=eq.${roomId.value}`,
            },
            (payload) => allMessages.value.push(payload.new)
        ).subscribe()
}

const addUser = async() => {
    const {data} = await supabase.from('users').select('id').eq('name', inputAdd.value)
    if(data != null && data != undefined){
        const insertInUsersRoom = await supabase.from('usersRoom').insert({'user': data[0].id, 'room': props.roomId})
    }
    inputAdd.value = ''
}

const getName = async() => {
    if(roomId.value != undefined){
        const {data} = await supabase.from('rooms').select().eq('id', roomId.value)
        nameOfGroup.value = data[0].name
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
    }
}

if(logged.value){
    onMounted(getMessages)
}
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
            <Channels @openRoom="openRoom" :activeUser="activeUser" :dark="dark"/>
        </div>

        <div class="w-full flex flex-col justify-items-stretch">
            
            <div :class="`${dark ? 'bg-gray-800' : 'bg-gray-700'} justify-between flex h-16 shadow-lg shadow-gray-900`">
                <TopBar :dark="dark" :name="nameOfGroup" @changeTheme="changeTheme"/>
            </div>
            <div class="mt-12 ml-12 flex">
                <button v-if="roomId != null" :class="`${dark ? 'bg-gray-800' : 'bg-gray-700'} left-0 text-slate-200 p-2 rounded-lg`" @click="addPop = !addPop">Add User</button>
                <input v-if="addPop && roomId != null" v-on:keyup.enter="addUser" v-model="inputAdd" class="bg-transparent border border-green-600 rounded-lg p-2 ml-4 placeholder-slate-400 text-slate-300 focus:text-white" placeholder="His username...">
                <button v-if="addPop && roomId != null" class="bg-gray-900 text-slate-200 py-2 px-4 ml-4 rounded-xl" @click="addUser">Add</button>
            </div>
            
            <div class="flex-col h-full kokot123 overflow-y-scroll">
                <TheChat :allMessages="allMessages" :roomId="roomId" :dark="dark"/>
            </div>

            <div class="flex items-center justify-center h-16 mt-8" v-if="roomId != null">
                <input v-model="message" v-on:keyup.enter="sendMessage" :class="`${dark ? 'text-gray-200 border-other-100 focus:text-white focus:bg-other-400 focus:ring-gray-800 focus:placeholder-gray-100' : 'text-white border-white focus:text-white focus:bg-gray-700 focus:placeholder-gray-100 border'} placeholder-gray-300 max-w-6xl w-full p-3 border rounded-xl flex-col justify-end bg-transparent`" placeholder="Chat with other people..."/>
                <PaperAirplaneIcon class="h-7 w-7 text-gray-200 inline-block -ml-10 hover:cursor-pointer hover:text-green-500" @click="sendMessage"/>
            </div>
        
        </div>
    </div>
</template>

<style>

body:not(.dark){
    background-color: rgb(72, 77, 84);
}
body.dark {
    background-color: rgb(119, 122, 126);
}
    .kokot123{
        height: calc(100vh - 20rem);
    }
</style>

