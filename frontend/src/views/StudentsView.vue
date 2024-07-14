<template>
  
    <div class = "table">
        <EasyDataTable
                buttons-pagination
                :headers="headers"
                :items="controllerStore.students"
                table-class-name="customize-table"
                :search-value="searchValue"
                >
                <template #item-image="{image}">
                    <img class = "table-student-image" :src="image" alt="">
                </template>
            </EasyDataTable>
    </div>
</template>

<script setup>
    import axios from "axios";
    import { onMounted, ref } from "vue";
    const searchValue = ref('')

    const headers = [
        { text: "Id", value: "id", width : 30},
        { text: "Rasm", value: "image", width: 50},
        { text: "Ism", value: "first_name", width: 150},
        { text: "Familiya", value: "last_name", width: 150},
        { text: "Fakultet", value: "faculty", width: 100},
        { text: "Telefon", value: "phone", width: 100},
        { text: "Kurs", value: "course", width: 100},
        { text: "...", value: "detail", width: 20 },
        
        
    ]

    import {useControllerStore} from '../stores/controller'

    const controllerStore = useControllerStore()


    onMounted(()=>{
        axios.get('allstudents/')
            .then((data)=>{
                controllerStore.students = data.data
            })
    })

</script>
