<script setup>
import {onMounted, ref} from 'vue'
import {supabase} from '../supabase'

const username = ref("")
const password = ref("")
const users = ref([])


const emits = defineEmits(['login', 'goSignup'])

const login = () => {
    for(let i=0; i<users.value.length; i++) {
        if(users.value[i].name == username.value && users.value[i].password == password.value){
            emits('login', users.value[i].id)
        }
    }
}

const getUser = async() => {
    const {data} = await supabase.from('users').select()

    users.value = data
}

onMounted(getUser)

</script>

<template>
    <div class="max-w-screen w-full text-center">
        <button class="text-white bg-green-600 w-32 h-12 rounded-lg mt-10 font-bold text-lg" @click="emits('goSignup')">Go Signup</button>
    </div>
    <div class="mx-auto mt-20 bg-gray-800 max-w-2xl rounded-2xl p-14">
        <span class="text-3xl text-slate-100 font-bold">Login</span>
        <label for="username" class="flex text-slate-100 mt-10">Username</label>
        <input v-model="username" id="password" class="bg-transparent w-full mt-1 p-3 border border-green-500 rounded-lg focus:text-black text-slate-100 focus:bg-blue-50 focus:placeholder-green-600 placeholder-slate-400" placeholder="Username..."/>
        <label for="password" class="flex text-slate-100 mt-5">Password</label>
        <input v-model="password" type="password" id="password" class="w-full mt-1 p-3 border border-green-500 rounded-lg focus:text-black text-slate-100 focus:bg-blue-50 focus:placeholder-green-600 bg-transparent placeholder-slate-400" placeholder="Password..."/>
        <button class="mt-10 ml-96 text-white bg-green-600 w-32 h-12 rounded-lg font-bold text-lg" @click="login">Login</button>
    </div>
</template>
