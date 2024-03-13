<template>
    <div class="flex flex-row flex-wrap w-auto box-border items-center gap-2 p-2">
        <div v-if="props.role == 'Lawyer'" class="flex flex-col  border-2 rounded-lg border-slate-200 items-center p-1">
            <div class="leading-5">Офис</div>
            <input v-model="filterData.office" type="number" min="1" @keypress="isNumber($event)" name="office" class="p-1 w-24"/>
        </div>
        <div class="h-16 flex flex-col border-2 rounded-lg border-slate-200 items-center p-1">
            <div class="leading-5">Менеджер</div>
            <input v-model="filterData.manager_id" type="number" min="1" @keypress="isNumber($event)" class="p-1 w-24"/>
        </div>
        <div class="h-16 flex flex-col border-2 rounded-lg border-slate-200 items-center p-1">
            <div class="leading-5">Юрист</div>
            <input v-model="filterData.laywer_id" type="number" min="1" @keypress="isNumber($event)" name="office" class="p-1 w-24"/>
        </div>
        <div class="h-16 flex flex-col border-2 rounded-lg border-slate-200 items-center p-1">
            <div class="leading-5">Клиент</div>
            <input v-model="filterData.client_id" type="number" min="1" @keypress="isNumber($event)" name="office" class="p-1 w-24"/>
        </div>
        <div class="h-16 flex flex-col border-2 rounded-lg border-slate-200 items-center p-1">
            <div class="leading-5">Месяц оплаты</div>
            <input v-model="filterData.date" type='date' name="office" placeholder="месяц оплаты" class="p-1 w-fit"/>
        </div>
        <div class="h-16 flex flex-col border-2 rounded-lg border-slate-200 items-center p-1">
            <div class="leading-5">Передача клиента</div>
            <div class="flex flex-row">
                <input v-model="filterData.dates_transfer[0]" type="date" class="p-1 pl-4 w-fit"/>
                <input v-model="filterData.dates_transfer[1]" :min="filterData.dates_transfer[0]" type="date" class="p-1 w-fit"/>
            </div>
        </div>
        <div class="flex flex-col border-2 rounded-lg border-slate-200 items-center p-1">
            <div class="leading-5">Дата оплаты</div>
            <div class="flex flex-row">
                <input v-model="filterData.dates_pay[0]" type="number" min="1" @keypress="isNumber($event)" placeholder="Начало" class="p-1 pl-4 w-24"/> 
                <input v-model="filterData.dates_pay[1]" type="number" :min="filterData.dates_pay[0]" @keypress="isNumber($event)" placeholder="Конец" class="p-1 pl-4 w-24"/> 
            </div>
        </div>        
    </div>
    <div class="flex flex-row flex-wrap w-auto box-border items-center gap-2 p-2">
        <button @click="filter()" class="w-36 h-16 flex flex-col justify-center rounded-lg bg-sky-500 text-center items-center p-1 hover:cursor-pointer hover:bg-sky-700 active:bg-sky-900 text-white font-bold">Отфильтровать</button>
        <button @click="resetFilter()" class="w-36 h-16 flex flex-col justify-center rounded-lg bg-gray-400 text-center items-center p-1 hover:cursor-pointer hover:bg-gray-600 active:bg-gray-800 text-white font-bold">Сбросить</button>
    </div>
</template>
<script setup>
    import { ref, reactive } from "vue";
    const urlParams = new URLSearchParams(window.location.search);    
    const props = defineProps({
        filters: {},
        role: String
    }) 
    const filterData = reactive({
        office : props.filters?.office || null,
        manager_id: props.filters?.manager_id || null,
        laywer_id: props.filters?.laywer_id || null,
        client_id: props.filters?.client_id || null,
        date: props.filters?.date || null,
        dates_transfer: props.filters?.dates_transfer || [],
        dates_pay: props.filters?.dates_pay || []

    })   
    import {Inertia} from "@inertiajs/inertia"
    let date_pay_1;
    let date_pay_2;
    function isNumber(event){
        event = (event) ? event : window.event;
        var charCode = (event.which) ? event.which : event.keyCode;
        if ((charCode > 31 && (charCode < 48 || charCode > 57)) && charCode !== 46) {
            event.preventDefault();;
        } else {
            return true;
        }
    }
    function filter(){
        let url = '/table?'
        for ( const [key, val] of Object.entries(filterData)){
            if (val) {
                if (Array.isArray(val) && val?.length>0) 
                    url += `${key}[]=${val[0]}&${key}[]=${val[1]}&`
                else if(!Array.isArray(val))
                    url+=`${key}=${val}&`
            }
        }
        Inertia.get(url)
    }
    function resetFilter(){
        Inertia.get('/table')
    }
</script>