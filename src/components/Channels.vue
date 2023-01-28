
<script setup>
    import {ref, onMounted} from 'vue'
    import { ChevronRightIcon, ChevronDoubleDownIcon, PlusIcon } from '@heroicons/vue/24/outline';
    import { supabase } from '../supabase';

    const supabaseChannels = ref([]);
    const openedChannel = ref()
    const channels = ref([])
    const inputName = ref('')
    const addDown = ref(false)
    const blankName = ref(false)

    const emits = defineEmits(['openRoom'])
    const props = defineProps(['activeUser', 'dark'])

    const getChannels = async() => {
        channels.value = []
        const {data} = await supabase.from('usersRoom').select().eq('user', props.activeUser)
        for(let i=0; i < data.length; i++) {
            channels.value.push(data[i].room)
        }
        getNames()
    }

    const getNames = async() => {
        supabaseChannels.value = []
        for(let i = 0; i < channels.value.length; i++) {
            const {data} = await supabase.from('rooms').select().eq('id', channels.value[i])
            supabaseChannels.value.push(data)
        }
    }
    
    const addChannel = async() => {
        if(inputName !== ''){        
            const {insertChannel} = await supabase.from('rooms').insert({'name': inputName.value})
            const {data} = await supabase.from('rooms').select()
            const {insertUser} = await supabase.from('usersRoom').insert({'user': props.activeUser, 'room': data.length})

            addDown.value = false
            blankName.value = false
            inputName.value = ''
        } else{
            blankName.value = true
        }
    } 

    onMounted(getChannels)

</script>


<template>
    <div class="mt-5">
        <span class="text-xl font-bold h-10">Channels</span>
        <button class="p-2 bg-green-600 w-24 rounded-xl mt-10 flex" @click="addDown = !addDown">
            <PlusIcon class="h-6 w-6 mx-auto"/>
        </button>
        <input v-model="inputName" v-if="addDown" :class="`${blankName ? 'bg-red-400' : 'bg-transparent'} w-32 mt-5 p-4 h-12 border border-green-500 rounded-xl text-white focus:placeholder:text-slate-300 focus:bg-other-600 focus:border-none`" placeholder="Name..."/>
        <button v-if="addDown" class="p-1 bg-green-600 w-16 h-8 rounded-xl mt-4 font-bold ml-8" @click="addChannel">Add</button>
        <div class="mt-16 overflow-y-scroll">
            <div v-for="channels in supabaseChannels" class="mt-5">
                <span v-for="channel in channels" :class="`flex hover:cursor-pointer p-2 rounded-md ${openedChannel == channel.id ? 'bg-other-100' : 'bg-other-600'}`" @click="emits('openRoom', channel.id), openedChannel = channel.id">
                    <ChevronDoubleDownIcon v-if="channel.id == openedChannel" class="h-5 w-5 mr-3 mt-0.5"/>
                    <ChevronRightIcon v-else class="h-5 w-5 mr-3 mt-0.5"/>
                    {{channel.name}}
                </span>
            </div>
        </div>
    </div>
</template>
