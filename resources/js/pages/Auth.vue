<template>
    <div class="w-screen h-screen flex justify-center items-center">
    <div class="form h-60 w-72 flex flex-col justify-center border-2 items-center gap-4 rounded-lg" >
        <input v-model="form.email" type="text" placeholder="name" class="w-52 h-8 p-1"/>
        <input v-model="form.password" type="password" placeholder="password" class="w-52 h-8 p-1"/>
        <button @click="submit" class="w-52 h-8 bg-sky-500 hover:bg-sky-700 active:bg-sky-900 rounded-lg text-white">Submit</button>
    </div>
</div>
</template>
<script setup>   
    import {Inertia} from "@inertiajs/inertia"
    import { reactive } from 'vue'
    const form = reactive({
        email : null,
        password: null
    })
    function submit(){
        Inertia.post('/login',form)
    }
</script>