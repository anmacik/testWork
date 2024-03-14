<template>
    <div class="w-screen h-screen flex justify-center items-center">
    <div class="form h-60 w-72 flex flex-col justify-center border-2 items-center gap-4 rounded-lg" >
        <input v-model="form.email" type="text" placeholder="name" class="w-52 h-8 p-1 focus-visible:outline-2 focus-visible:!outline-sky-500"  :class="{'border-2 rounded-lg outline-red-200' : redBorder.email }" @focus="redBorder.email=false"/>
        <input v-model="form.password" type="password" placeholder="password" class="w-52 h-8 p-1 focus-visible:outline-2 focus-visible:!outline-sky-500" :class="{'border-2 rounded-r-lg outline-red-200': redBorder.password }" @focus="redBorder.password=false"/>
        <div  v-if='loginError' class="text-red text-sm"> Не правильный логин/пароль</div>
        <button @click="submit" class="w-52 h-8 bg-sky-500 hover:bg-sky-700 active:bg-sky-900 rounded-lg text-white">Submit</button>
    </div>
</div>
</template>
<script setup>   
    import {Inertia} from "@inertiajs/inertia"
    import {ref, reactive } from 'vue'
    const form = reactive({
        email : null,
        password: null
    })
    const redBorder = reactive({
        email : false,
        password : false
    })
    const loginError = ref(false)

    function submit(){
        Inertia.post('/login',form, {
            onError: ( err)=>{
                console.log('err',err);
            },
            onSuccess: (result)=>{
                if (result.component == "Auth"){                    
                    redBorder.email = true;
                    redBorder.password = true;
                    loginError.value = true
                }
            }
        })
    }
</script>