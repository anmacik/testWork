<template>
        <div class="absolute z-50 top-0 left-0 w-screen h-screen flex flex-col items-center justify-center bg-gray-200 bg-opacity-25 border-r-4">
            <div class='border-2 bg-blue-50 border-sky-500 rounded-lg' ref="modalWindow">
                <div v-if="items.length ==0 ">Платежей не найдено</div>
                <div v-else class="flex flex-col items-center justify-center p-3" > Список платежей
                    <div v-for="item in items">
                    {{item.amount}} - {{ item.payed_at }}
                    </div> 
                </div>
            </div>
        </div>
</template>
<script setup>
    import {onMounted, onBeforeUnmount, ref} from "vue"
    const emit = defineEmits(['close']);
    const props = defineProps({
        items: Array,
    })
    
    const modalWindow = ref(null); 
    
    onMounted(()=> {
        document.documentElement.style.overflow = 'hidden'
        document.addEventListener("click", onClickOutside);
    });
    onBeforeUnmount(()=> {
        document.documentElement.style.overflow = 'auto'
        document.removeEventListener("click", onClickOutside);
    })
    function onClickOutside(event) {   
      if (event.target == modalWindow.value) return;
      emit('close');
    }
    
</script>   