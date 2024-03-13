<template>
    <header class="flex flex-row justify-end h-14 box-border items-center w-full bg-gray-800">
        <button @click="logOut()" class="w-32 text-white hover:text-blue-200 active:text-blue-400">Log out</button>
    </header>
</template>
<script setup>
    import {Inertia} from "@inertiajs/inertia"
    function logOut(){
        Inertia.post('/logout')
    }
</script>