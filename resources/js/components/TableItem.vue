<template>
    <tr>
        <td v-if="props.role == 'Lawyer'" class="border-2 font-normal border-sky-500 p-1">
            {{ props.client.manager.office }}-<br/>
            <select v-model="data.manager_id" @change="changed=true">
                <option v-for="(name, id) in props.managers" :value="id">{{ name }}</option>
            </select>                             
        </td>
        <td v-else class="border-2 font-normal border-sky-500 p-1">{{props.client.manager.name}} </td>
        <td class="border-2 font-normal border-sky-500 p-1">
            <select v-model="data.lawyer_id" @change="changed=true">
                <option v-for="(name, id) in props.lawyers" :value='id'>{{name}}</option>
            </select>
        </td>
        <td class="border-2 font-normal border-sky-500 p-1">
            <input type='text' v-model='data.name' @input="changed=true">
        </td>
        <td v-if="props.role =='Lawyer'" class="border-2 font-normal border-sky-500 p-1">{{props.client.program_type}} </td>                
        <td class="border-2 font-normal border-sky-500 p-1">{{props.client.date_register}} </td>
        <td class="border-2 font-normal border-sky-500 p-1">{{props.client.program_plan}}/{{props.client.all_payments}} </td>
        <td class="border-2 font-normal border-sky-500 p-1">{{props.client.program_price}}/{{props.client.all_payments}} </td>
        <td class="border-2 font-normal border-sky-500 p-1" @click.prevent.stop="$emit('showModal',props.client.list_full_remmitances)">{{props.client.program_price}}/{{props.client.all_payments}}</td>
        <td class="items-center border-2 font-normal border-sky-500 p-1">
            <div class="h-16 flex flex-row gap-1 justify-around">
                <input v-model="data.first_date" @input="changed=true" type="number" min="1" class="w-6 border-0"  /> 
                <input v-model="data.second_date" @input="changed=true" type="number" :min="props.client.program_first_date" class="w-6 border-0" /> 
            </div>
        </td>
        <td class="border-2 font-normal border-sky-500 p-1">{{props.client.client_handover_date}} </td>
        <td v-if="changed" @click="updateData" class="rounded-xl text-center p-2 bg-clip-content cursor-pointer bg-sky-500 text-white">Обновить</td>
        <td v-else class="text-center bg-gray-200 rounded-xl cursor-not-allowed p-2 bg-clip-content">Обновить</td>       
    </tr>
</template>
<script setup>
import {ref, reactive} from "vue";
import {Inertia} from "@inertiajs/inertia";
const props = defineProps({
    client : {},
    managers : {},
    lawyers : {},
    role : String
})
const data = reactive({
    id : props.client?.id,
    name : props.client?.client_name || null,
    manager_id: props.client?.manager?.id || null,    
    lawyer_id : props.client?.lawyer?.id || null,
    first_date : props.client?.program_first_date || null,
    second_date : props.client?.program_second_date || null,
})
const changed = ref(false)
function updateData(){
    Inertia.post('/update-data', data,{
        onSuccess: (result)=>{
            console.log('успех');
            console.log(result)
        }
    })
}
</script>