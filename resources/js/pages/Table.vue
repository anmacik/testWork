<template>
    <Header/>
    <div class="flex flex-col gap-3  p-4">
        <TableFilter :filters="props.filters" :role="props.role"/> 
        <div class="flex flex-col gap-1">
            <div class="flex flex-row justify-end">
                <div v-for="page in pages">
                    <div v-if="page!=props.payPlanItem.current_page" @click="openPage" class="h-8 w-8 p-1 text-center bg-sky-500 hover:cursor-pointer text-white">{{ page }}</div>
                    <div v-else class="h-8 w-8 p-1 text-center bg-gray-500 text-gray-400 hover:cursor-default" >{{ page }}</div>
                </div>
            </div>
            <div class="overflow-x-auto">
                <table class="table-auto w-full border-collapse border-2 ">
                    <thead>
                        <tr>
                            <th v-if="props.role == 'Lawyer'" class="border-2 bg-blue-50 font-medium w-max border-sky-500 p-1" >Офис и менеджер</th>
                            <th v-else class="border-2 bg-blue-50 font-medium  w-max border-sky-500 p-1" >Офис</th>
                            <th class="border-2 bg-blue-50 font-medium  w-max border-sky-500 p-1">Юрист</th>
                            <th class="border-2 bg-blue-50 font-medium  w-max border-sky-500 p-1">Фио</th>
                            <th v-if="props.role == 'Lawyer'" class="border-2 bg-blue-50 font-medium  w-max border-sky-500 p-1">Программа</th>
                            <th class="border-2 bg-blue-50 font-medium  w-max border-sky-500 p-1">Дата регистрации</th>
                            <th class="border-2 bg-blue-50 font-medium  w-max border-sky-500 p-1">План</th>
                            <th class="border-2 bg-blue-50 font-medium  w-max border-sky-500 p-1">Стоимость программы</th>
                            <th class="border-2 bg-blue-50 font-medium  w-max border-sky-500 p-1">В месяц</th>
                            <th class="border-2 bg-blue-50 font-medium  w-max border-sky-500 p-1">Дата оплаты</th>
                            <th class="border-2 bg-blue-50 font-medium  w-max border-sky-500 p-1">Дата передачи</th>
                        </tr>
                    </thead>
                    <tbody>
                        <TableItem v-for="client in props.payPlanItem.data" :client="client" :role="props.role" :managers="props.managers" :lawyers="props.lawyers" @showModal="(childItems)=>{showModal=true;modalItems=childItems }"/>
                    </tbody>
                </table>
            </div>
        </div>
    </div>
    <Modal v-if="showModal" :items="modalItems" 
    @close="showModal=false" 
    @wheel.prevent
    @touchmove.prevent
    @scroll.prevent/>    
</template>
<script setup>
    import Header from './../components/Header.vue';
    import TableFilter from './../components/TableFilter.vue';
    import Modal from './../components/Modal.vue'
    import TableItem from './../components/TableItem.vue'
    import {Inertia} from "@inertiajs/inertia"
    import {ref, reactive} from "vue"
    const testForm = ref(null)
    const props = defineProps(['payPlanItem', 'filters', 'role', 'offices', 'managers', 'lawyers']);
    const showModal = ref(false);   
    const changed = reactive(props.payPlanItem.data.reduce((acc, cur)=>({...acc, [cur.id]:false}),{}))
    const modalItems = ref([]);
    let pages;
    if (props.payPlanItem.last_page >1) pages = Array.from({length: props.payPlanItem.last_page},(value, index) => index+1 )  

    function openPage(event){
        Inertia.get(`${filterUrl()}&page=${event.target.innerHTML}`)
    }

    function filterUrl(){
        let url = '/table?'
        for ( const [key, val] of Object.entries(props.filters)){
            if (val) {
                if (Array.isArray(val) && val?.length>0) 
                    url += `${key}[]=${val[0]}&${key}[]=${val[1]}&`
                else if(!Array.isArray(val))
                    url+=`${key}=${val}&`
            }
        }
        return url;
    }
    function openModal(data){
        console.log(data);
    }
</script>