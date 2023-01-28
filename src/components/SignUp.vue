<script setup>
import { supabase } from '../supabase';
import {ref} from 'vue'


const username = ref('')
const password = ref('')
const secPassword = ref('')
const passNotSame = ref(false)
const blank = ref(false)
const signupped = ref(false)


const emits = defineEmits(['goLogin'])

const signUp = async() => {
    if(password.value == secPassword.value && password.value !== '' && secPassword.value !== '' && username.value !== ''){
        const {data} = await supabase.from('users').insert({'name': username.value, 'password': password.value});
        emits('signup')
        passNotSame.value = false
        blank.value = false
        signupped.value = true
    } else{
        if(password.value !== secPassword.value){
            passNotSame.value = true
        } else{
            blank.value = true
        }
    }
}


</script>

<template>
    <div class="max-w-screen w-full text-center">
        <button class="text-white bg-green-600 w-32 h-12 rounded-lg mt-6 font-bold text-lg" @click="emits('goLogin')">Go Login</button>
    </div>
    <div class=" mx-auto bg-gray-800 max-w-2xl rounded-2xl p-14 mt-8">
        <span class="text-3xl text-slate-100 font-bold">SignUp</span>
        <label for="username" class="flex text-slate-100 mt-10">Username</label>
        <input v-model="username" id="password" class="bg-transparent w-full mt-1 p-3 border border-green-500 rounded-lg focus:text-black text-slate-100 focus:bg-blue-50 focus:placeholder-green-600 placeholder-slate-400" placeholder="Username..."/>
        <label for="password" class="flex text-slate-100 mt-5">Password</label>
        <input v-model="password" type="password" id="password" :class="`${passNotSame ? 'bg-red-400' : 'bg-transparent'} w-full mt-1 p-3 border border-green-500 rounded-lg focus:text-black text-slate-100 focus:bg-blue-50 focus:placeholder-green-600 bg-transparent placeholder-slate-400`" placeholder="Password..."/>
        <label for="password" class="flex text-slate-100 mt-5">Password again</label>
        <input v-model="secPassword" type="password" id="password2" :class="`${passNotSame ? 'bg-red-400' : 'bg-transparent'} w-full mt-1 p-3 border border-green-500 rounded-lg focus:text-black text-slate-100 focus:bg-blue-50 focus:placeholder-green-600 bg-transparent placeholder-slate-400`" placeholder="One more time please..."/>
        <span v-if="passNotSame" class="text-red-500 text-xl">Passwords aren't same</span>
        <span v-if="blank" class="text-red-500 text-xl">It can't be blank</span>
        <button class="mt-10 ml-96 text-white bg-green-600 w-32 h-12 rounded-lg font-bold text-lg" @click="signUp">SignUp</button>
    </div>
</template>
