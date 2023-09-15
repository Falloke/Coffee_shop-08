<script setup>
import { ref,computed  } from 'vue';


//variable drinking
const travelList=ref([
  {name: 'Café de Thyme',  price:500, img:"src/img/a745ee2d-a168-4e10-a2de-008fbf00cd87.jpg", quantity: 0 }, 

  {name:'Misstarbydavika',  price:750, img:"src/img/13e4ae51-2a9b-4761-9c5a-d675dffd7413.jpg", quantity: 0 },
  
  {name:'TAground Cafe And Restaurant',      price:1500, img:"src/img/03.jpg", quantity: 0 },

  {name:'Esta cafe & Co working space',     price:1599, img:"src/img/04.jpg", quantity: 0 },

  {name:'minimalcafe.bkk',    price:1699, img:"src/img/05.jpg", quantity: 0 },


  {name:'E.NIF',  price:1890,  img:"src/img/06.jpg", quantity: 0 },


  {name:'Honey Coffee and Tea',      price:675, img:"src/img/07.jpg", quantity: 0 },


  {name:'Time 2 Time cafe&gallery',     price:1250, img:"src/img/08.jpg", quantity: 0 },
])


//เก็บ list การจอง 
const list_local = ref([


]);


//function
function list_local_control(name, quantity) {
  const existingItemIndex = list_local.value.findIndex(item => item.name === name);

  if (existingItemIndex !== -1) {
    list_local.value[existingItemIndex].quantity += quantity;
    list_local.value[existingItemIndex].price = list_local.value[existingItemIndex].quantity * travelList.value.find(item => item.name === name).price;
  } else {
    const item = {
      name: name,
      quantity: quantity,
      price: quantity * travelList.value.find(item => item.name === name).price
    };
    list_local.value.push(item);
  }
}


function incrementQuantity(item) {
  item.quantity++;
  item.price = item.quantity * travelList.value.find((travel) => travel.name === item.name).price;
}

function decrementQuantity(item) {
  if (item.quantity > 0) {
    item.quantity--;
    item.price = item.quantity * travelList.value.find((travel) => travel.name === item.name).price;
  }
}

function removeItem(index) {
  list_local.value.splice(index, 1);
}



//pop up

const form_costumer = ref({
  name: "",
  phone: "",
  date: "",
  time: ""
});

const showPopup = ref(false);

const totalCost = computed(() => {
    return list_local.value.reduce((acc, item) => acc + item.price, 0);
});

function displayPopup() {
    showPopup.value = true;
}

function togglePopup() {
  showPopup.value = !showPopup.value;
}


</script>



<template>

  <HelloWorld v-if="showPopup" />
  <div class="overlay" v-if="showPopup">
    <HelloWorld />
  </div>

  <div class="header">
    <H1>เลือกร้านที่ชอบๆ</H1>
  </div><hr>


  <div class="container text-center">
    <div class="row">
      <div class="col" v-for="(i,index) in travelList" :key="index">
        <div class="card" style="width: 18rem;" >
          <img :src="i.img" class="card-img-top">
          <div class="card-body">
            <h3 class="card-title">{{ i.name }}</h3>
            <p>ราคาโต๊ะ : {{i.price}}</p>
            <h6>***จำกัด 5 คนต่อ 1 โต๊ะ***</h6>
            <p class="card-text">จำนวนโต๊ะ 
               <input type="number" class="cout_value " v-model="i.quantity">
            </p>
            <a class="btn btn-primary" @click="list_local_control(i.name, i.quantity)"> จองโต๊ะ </a>
          </div>
        </div>
      </div>
    </div>
  </div> 

 

  <h1 style="text-align: center; " v-if="list_local.length>0" class="header">รายการจองโต๊ะ</h1>
  <div class="list_cout text-center">
    <table class="table table-hover" v-if="list_local.length>0">
      <table class="table">
        <thead>
          <tr>
            <th scope="col">ลำดับ</th>
            <th scope="col">ชื่อร้าน</th>
            <th scope="col">จำนวนโต๊ะ</th>
            <th scope="col">ราคา</th>
            <th scope="col"></th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(i,index) in list_local" :key="index" >
            <th scope="row">{{index+1}}</th>
            <td>{{ i.name }}</td>
            <td>
              <div class="quantity-controls">
                <button type="button" class="btn btn-danger"   @click="decrementQuantity(i)" :disabled="i.quantity === 0"> - </button>
                <span>{{ i.quantity }}</span>
                <button type="button" class="btn btn-success" @click="incrementQuantity(i)">+</button>
              </div>
            </td>
            <td>{{ i.price}}</td>
            <td>
              <button type="button" class="btn btn-danger" @click="removeItem(index)" >ลบรายการ</button>
            </td>
          </tr>
        </tbody>
      </table>  
    </table>
  </div>  

  
  <div class="contrainer_costumer " v-if="list_local.length>0">
  <div class="form_box"> <h2 style="text-align: center;">กรุณากรอกข้อมูล</h2></div>
  <form>
    <div class="form_box">
      <label for="name">ชื่อ-นามสกุล </label><br>
      <input type="text" id="name" class="form-control" v-model="form_costumer.name" required>
    </div>
    <div class="form_box">
      <label for="phone">เบอร์โทร </label><br>
      <input type="tel" id="phone" class="form-control" v-model="form_costumer.phone" required>
    </div>
    <div class="form_box">
      <label for="date">วันที่ </label><br>
      <input type="date" id="date" class="form-control" v-model="form_costumer.date" required>
    </div>
    <div class="form_box">
      <label for="time">เวลา </label><br>
      <input type="time" id="time" class="form-control" v-model="form_costumer.time" required>
    </div>
    <div class="form_box">
      <button type="button" class="btn btn-success" @click="togglePopup"> ยืนยันการจอง </button>
    </div>
  </form>
</div>


<div v-if="showPopup" class="overlay">

  <div class="popup">
      <h2 style="text-align: center; color:green ;">ยืนยันการจองสำเร็จ</h2>
      <h3 >รายละเอียด</h3>
      <p><strong>ชื่อ-นามสกุล:</strong> {{ form_costumer.name }}</p>
      <p><strong>เบอร์โทร:</strong> {{ form_costumer.phone }}</p>
      <p><strong>วันที่:</strong> {{ form_costumer.date }}</p>
      <p><strong>เวลา:</strong> {{ form_costumer.time }}</p>
      
      <table class="box_table">
          <thead>
              <tr>
                  <th>ลำดับ</th>
                  <th>ชื่อร้าน</th>
                  <th>จำนวนโต๊ะ</th>
                  <th>ราคา</th>
              </tr>
          </thead>
          <tbody>
              <tr v-for="(i, index) in list_local" :key="index">
                  <td>{{ index + 1 }}</td>
                  <td>{{ i.name }}</td>
                  <td>{{ i.quantity }}</td>
                  <td>{{ i.price }}  บาท </td>
              </tr>
          </tbody>
      </table>

      <p><strong>รวม:</strong> {{ totalCost }} บาท</p>

      <button class="btn btn-danger" @click="showPopup = false">ปิดหน้าต่าง</button>
  </div>
</div>



</template>


<style>
*{
  padding: 5px;
}
.header{
  text-align: center;
}
.card-img-top{
  height: 100%;
  padding: 25px;
}

.col{
  padding: 10px;
}


.contrainer_customer {
  justify-content: center;
  align-items: center;
  margin-left: 20%;
  margin-right: 20%;

}

.form_box{
    justify-content: center;
    align-items: center;
    padding: 5px;
    margin-left: 30%;
    margin-right: 30%;
}


.overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.7);
  display: flex;
  justify-content: center;
  align-items: center;
}


.popup {
  background-color: #fff;
  padding: 20px;
  border-radius: 10px;
  width: 70%;
  max-height: 80%;
  overflow-y: auto;
}

.header{
  padding: 25px;
}

</style>