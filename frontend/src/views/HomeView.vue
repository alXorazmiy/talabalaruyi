
<template>
    <div class = 'container'>
        <div class="box">
            <div class="buttons">
                
                <n-popconfirm
                    :show-icon="false"
                    style="background-color: var(--color-white);"                 
                >
                    <template #trigger>
                        <n-button type = "error" class = "table-delete-button"  strong secondary >Qavat qo'shish</n-button>
                    </template>
                    <template #action>
                        <button class = 'popconfirm-button' @click="create_floor">Ha</button>
                    </template>
                    <div  style="color: var(--color-dark)">
                        Qavat qo'shishni xoxlaysizmi? 
                    </div>
                </n-popconfirm>
            </div>
            <div class="floors">
                <swiper  class="swipper"
                    :modules="modules"
                    update-on-window-resize
                    :slides-per-view="auto"
                    :space-between="swiper_item == false ? 40 : 0"
                    navigation
                    
                >
               
                    <swiper-slide class = "swipper-slide" v-for="floor in controllerStore.floors" :key = "floor" @click="click_floor(floor.number)" > 
                        <div class = 'swipper-item' :class="floor_active == floor.number? 'floor-active': ''">
                            <div class = 'swipper-item-number'>{{floor.number}}  - qavat</div>
                            <div class = 'swipper-item-info'>
                                <i class="fa-solid fa-circle"></i>
                                <p> Bo'sh Joy: {{ floor.empty_place }}</p>
                            </div>
                            <div class = "swipper-item-info2">
                                <i class="fa-solid fa-circle"></i>
                                <p>Band Joy: {{ floor.busy_place }}</p>
                            </div>
                        </div>
                    </swiper-slide>
                </swiper>
            </div>
            <div class="rooms">
                <div class="floorborder">
                    <div class="room" v-for="room in controllerStore.rooms" :key ="room">
                        <i class="fa-solid fa-bed" :class = "room.beds[0] !=false ? 'room-busy': ''" @click = "show_bed(room, 1, room.beds[0])"></i>
                        <i class="fa-solid fa-bed" :class = "room.beds[1] !=false ? 'room-busy': ''" @click = "show_bed(room, 2, room.beds[1])"></i>
                        <i class="fa-solid fa-bed" :class = "room.beds[2] !=false ? 'room-busy': ''" @click = "show_bed(room, 3, room.beds[2])"></i>
                        <i class="fa-solid fa-bed" :class = "room.beds[3] !=false ? 'room-busy': ''" @click = "show_bed(room, 4, room.beds[3])"></i>
                        <img class = "computer" src="../assets/images/computer.png" alt="">
                        <img class ="table" src="../assets/images/table.png" alt="">
                    </div>
                    <!-- <div class = 'stair'>
                        <img src="../assets/images/stairs.png" alt="">
                    </div> -->
                </div>
            </div>
        </div>

        <n-modal
                v-model:show="show_if"
                :auto-focus = 'false'
            >
            <div class = 'modal'>
                <div class = 'room-info'>
                    <p>{{ floor_active}} - qavat {{ room.number }} -xona {{ number }} - joy </p>
                </div>
                
                <div class = 'student'>
                    <div class="student-image">
                        <label for="studentImage" v-if="user_image ==''"><img src="../assets/images/user.png" alt=""></label>
                        <input type="file" id = "studentImage" @change="fileSelected">
                        <img :src="user_image"  v-if ="user_image !=''">
                    </div>
                    <form @submit.prevent="create_student">
                    
                        <div class = "modal-item">
                            <p >Ism</p>
                            <input type="text"  v-model="student.name">
                        </div>
                        <div class = "modal-item">
                            <p >Familiya</p>
                            <input type="text"  v-model="student.last_name">
                        </div>
                        <div class = "modal-item">
                            <p >Kurs</p>
                            <input type="number"  v-model="student.course">
                        </div>
                        
                        <div class = "modal-item">
                            <p >Fakultet</p>

                            <div>
                                <select v-model="student.faculty" placeholder="Fakultet">
                                    <option :value = "faculty.id" v-for="faculty in controllerStore.faculty" :key = "faculty">{{ faculty.name }} </option>
                                </select>
                                <n-popconfirm v-model:show ="popconfirm_if"  :negative-text = 'null' :positive-text = 'null' :show-icon="false" style = "background-color: var(--color-white)">
                                    <template #activator>
                                        <i class="fa-solid fa-plus"></i>
                                    </template>
                                    <div>
                                        <p>
                                            <input type="text" placeholder="Fakultet nomi" v-model="faculty_name">
                                        </p>
                                        <div class="ponconfirm-buttons">
                                            <button @click="create_faculty">Yaratish</button>
                                        </div>
                                    </div>
                                </n-popconfirm>

                            </div>
                        </div>
                        <div class = "modal-item">
                            <p >Sana</p>
                            <input type="text"  v-model="student.date">
                        </div>

                    </form>

                </div>


                <div class="student-payments" v-if = "student_if">
                    <h3 >To'lovlar</h3>
                    <hr>
                    <div class="payment-item" v-for="month in controllerStore.months" :key = "month">
                        <p class = 'payment-date'>{{ month.month }}-{{ month.year }}</p>
                        <p class = "payment-amount" v-if = "month.status == 'payment'">
                            100 000 <span>UZS</span>
                        </p>
                        <p  class = "payment-amount" v-if = "month.status != 'payment'">
                            0 <span>UZS</span>
                        </p>

                        <p class = "payment-status status-success" v-if = "month.status == 'payment'" >
                            To'langan
                        </p>
                        <p  class = "payment-status status-danger" v-if = "month.status != 'payment'" >
                            To'lanmagan
                        </p>


                    </div>

                </div>

                <div class="buttons">
                    <button class = 'cancel' @click="cancel">Bekor qilish</button>
                    <button type="submit" class = 'save' @click="create_student">Saqlash</button>
                </div>
            </div>

        </n-modal>



       

    </div>
</template>


<script setup>
    import axios from 'axios'
    import { Swiper, SwiperSlide } from 'swiper/vue'
    import { Navigation, Pagination, Scrollbar, A11y } from 'swiper/modules'

    import { onMounted,ref } from 'vue'
    import { useControllerStore } from '@/stores/controller'

    const controllerStore = useControllerStore()

    const floor_active = ref(1)
    const show_if = ref(false)
    const popconfirm_if = ref(false)
    const student_if =ref(false)
  


    const faculty_name = ref('')
    const user_image = ref('')
    const room = ref()
    const number = ref()
    const student = ref({
        name: "",
        last_name : "",
        course: '',
        faculty : '',
        date: ""
    })

    const modules = [
        Navigation,
        Pagination
    ]

    function cancel(){
        show_if.value = false
    }
    function click_floor(number){
        floor_active.value = number
        axios.get(`rooms/${floor_active.value}/`)
            .then((data)=>{
                getData()
                controllerStore.rooms = data.data
            })
    }

    
    function create_floor(){
        axios.post('floor/')
            .then((data)=>{
                controllerStore.floors = data.data
            })
    }

    function create_faculty(){
        let request = {
            name : faculty_name.value
        }
        axios.post('faculty/', request)
            .then((data)=>{
                controllerStore.faculty = data.data
                popconfirm_if.value = false
                faculty_name.value = ''
            })
    }
    function create_student(){
        let sana = student.value.date
        let day = sana.slice(0,2)
        let month = sana.slice(3,5)
        let year = sana.slice(6,11)
        let new_date = new Date(`${year}-${month}-${day}`)
        let date = new_date.toISOString()
        let request = {
            floor : room.value.floor.number,
            room : room.value.number,
            bed: number.value,
            first_name: student.value.name,
            last_name : student.value.last_name,
            course : student.value.course,
            faculty : student.value.faculty,
            image : user_image.value,
            date : date
        }
        axios.post('student/', request)
            .then((data)=>{
                if(data.status == 200){
                    show_if.value = false
                    getData()
                }
            })
    }

   
    function fileSelected(e){
        const fd = new FormData()
        fd.append('image', e.target.files[0])
        axios.post('image/', fd)
            .then((data)=>{
                user_image.value = data.data.image
            })
    }

    function allMonths(){
        let payments = controllerStore.payments
        let summa = 0
        for(let i = 0; i < payments.length; i ++) {
            summa = summa + payments[i].amount
        }
        console.log(summa / 100000)
        let payment_month = summa / 100000
        
        let start_month = student.value.date.slice(3,5)
        let start_year = student.value.date.slice(6,11)
        let end_month = new Date().toLocaleDateString('RU-ru').slice(3,5)
        let end_year = new Date().toLocaleDateString('RU-ru').slice(6,11)
        let list_month = []
        let list_month_name =['Yanvar', 'Fevral','Mart', "Aprel",'May','Iyun','Iyul','Avgust','Sentabr','Oktabr',"Noyabr",'Dekabr']
        let between_month = (Number(end_year)* 12 + Number(end_month))-(Number(start_year)* 12 + Number(start_month))

        for (let i = 0; i < Number(between_month) + 1; i++){
            let date = {}
            if (i >= payment_month){
                date = {
                    month : list_month_name[Number(start_month) + i - 1],
                    year: start_year,
                    status: 'no_payment'
                }
            }
            else{
                date = {
                    month : list_month_name[Number(start_month) + i - 1],
                    year: start_year,
                    status: "payment"
                }
            }
            list_month.push(date)
        }
        controllerStore.months = list_month
    }

    function show_bed(room_, number_, student_id){
        show_if.value = true
        room.value = room_ 
        number.value = number_
        if(student_id == false){
            student_if.value = false
            student.value.name = ''
            student.value.last_name = ''
            user_image.value = ''
            student.value.course = ''
            student.value.faculty = ''
            student.value.date = ''
            axios.get('faculty/')
                .then((data)=>{
                    controllerStore.faculty = data.data
                })
        }
        else{
            student_if.value = true
            axios.get(`student/?student=${student_id}`)
                .then((data)=>{
                    student.value.name = data.data.first_name
                    student.value.last_name = data.data.last_name
                    user_image.value = data.data.image
                    student.value.course = data.data.course
                    student.value.faculty = data.data.faculty
                    student.value.date = new Date(data.data.in_time).toLocaleDateString('RU-ru')
                })
            axios.get('faculty/')
                .then((data)=>{
                    controllerStore.faculty = data.data
                })

            axios.get(`payments/?student=${student_id}`)
                .then((data)=>{
                    controllerStore.payments = data.data
                    allMonths()

                })
        }
    }
    function getData(){
        axios.get('floor/')
            .then((data)=>{
                controllerStore.floors = data.data
            })

        axios.get(`rooms/${floor_active.value}/`)
            .then((data)=>{
                controllerStore.rooms = data.data
            })
    }

    onMounted(()=>{
          
        getData()
    })



</script>