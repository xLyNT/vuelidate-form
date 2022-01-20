<template>

<div class="container">

  <Popup v-if="isSubmited" heading="Форма успешно заполнена" :color="'green'" :isActive = "isSubmited"/>
  <Popup v-if="isWrong" heading="Пожалуйста, заполните следующие обязательные поля:" :color="'red'" :isActive = "isWrong" :fieldsToShow="this.fieldsToShow" />


  <h1>Заполните форму создания клиента</h1>
  <p>Поля отмеченные звездочкой обязательны для заполнения</p>
  <form class="client-form" @submit.prevent="onSubmit">

    <div class="client-form-main">

      <div class="input-container">

        <label for="lastName" >Фамилия<span>*</span></label>
        <input type="text" id="lastName" v-model="lastName">
      
      </div>

      <div class="input-container">

        <label for="firstName">Имя<span>*</span></label>
        <input type="text" id="firstName" v-model="firstName">

      </div>
      <div class="input-container">

        <label for="famName">Отчество</label>
        <input type="text" id="famName" v-model="famName">
      
      </div>

      <div class="input-container">

        <label for="birthDate">Дата рождения<span>*</span></label>
        <input type="date" id="birthDate" v-model="birthDate">

      </div>

      <div class="input-container">

        <label for="phoneNum">Номер телефона<span>*</span></label>
        <input type="tel" id="phone-number" name="phoneNum" v-model="phoneNum" >
      
      </div>

      <div class="input-container">

        <label for="gender">Пол</label>

        <div class="input-container gender-container"> 
          <input type="radio" id="gender-male" name="gender" value="male" v-model="gender" >
          <label for="gender-male">Мужской</label>

          <input type="radio" id="gender-female" name="gender" value="female" v-model="gender">
          <label for="gender-female">Женский</label>
        </div>

      </div>

      <div class="input-container client-groups">

      <label for="clientGroups" >Группа клиентов<span>*</span></label>

        <select multiple="true" size="3" v-model="clientGroups">
          <option value="VIP">VIP</option>
          <option value="Problem">Проблемные</option>
          <option value="OMS">ОМС</option>
        </select>

      </div>

      <div class="input-container">
        
        <label for="client-groups">Лечащий врач</label>

        <select size="1" v-model="doctor">
          <option value="">Лечащий врач</option>
          <option value="Ivanov">Иванов</option>
          <option value="Zaharov">Захаров</option>
          <option value="Chernysheva">Чернышева</option>
        </select>

      </div>

      <div class="input-container">

        <label for="noSms">Не отправлять СМС</label> <input  type="checkbox" id="noSms" v-model="noSms">

      </div>
  </div>

  <div class="client-form-address">

    <div class="input-container">

      <label for="index">Индекс</label>
      <input type="number" id="index" v-model="index">

    </div>

   <div class="input-container">
      <label for="country">Страна</label>
      <input type="text" id="country" v-model="country">
  </div>

    <div class="input-container">
      <label for="area">Область</label>
      <input type="text" id="area" v-model="area">
    </div>

    <div class="input-container">
      <label for="city">Город<span>*</span></label>
      <input type="text" id="city" v-model="city">
    </div>

    <div class="input-container">
      <label for="street">Улица</label>
      <input type="text" id="street" v-model="street">
    </div>

    <div class="input-container">
      <label for="house">Дом</label>
      <input type="text" id="house" v-model="house">
    </div>

  </div>

  <div class="client-form-passport">
    <div class="input-container">
      <label for="client-groups">Тип документа<span>*</span></label>

          <select size="1" v-model="docType">
           <option value="passport">Паспорт</option>
           <option value="birth">Свидетельство о рождении</option>
           <option value="driver">Вод. удостоверение</option>
          </select>
    </div>

    <div class="input-container">
      <label for="serial">Серия</label>
      <input type="number" id="serial" v-model="serial">
    </div>

    <div class="input-container">
      <label for="passNum">Номер</label>
      <input type="number" id="passNum" v-model="passNum">
    </div>

    <div class="input-container">
      <label for="passFrom">Кем выдан</label>
      <input type="text" id="passFrom" v-model="passFrom">
    </div>

    <div class="input-container">
      <label for="passDate">Дата выдачи<span>*</span></label>
      <input type="date"  id="passDate" v-model="passDate">
    </div>
  </div>

  <input type="submit" value="Создать клиента" class="btn">
  </form>

</div>
</template>

<script>
import useVuelidate from '@vuelidate/core';
import { required, minLength, maxLength} from '@vuelidate/validators';
import Popup from './components/Popup.vue';


export default {
  setup(){
    return { v$:useVuelidate()};
  },
  data(){
    return{
      lastName:'',
      firstName:'',
      famName:'',
      birthDate:'',
      phoneNum:'7',
      gender:'',
      clientGroups:[],
      doctor:'',
      noSms:'',

      index:'',
      country:'',
      area:'',
      city:'',
      street:'',
      house:'',

      docType:'',
      passFrom:'',
      serial:'',
      passNum:'',
      passDate:'',

      fieldsToShow:[],
      isSubmited:false,
      isWrong:false
    }
  },
  validations(){
    return {
      lastName:{required},
      firstName:{required},
      birthDate:{required},
      phoneNum:{required,minLength:minLength(11), maxLength:maxLength(11)},

      clientGroups:{required},

      city:{required},

      docType:{required},
      passDate:{required}

    }
  },
  name: 'App',
  components: {
    Popup,
  },
  methods:{
    onSubmit(){
      if(isNaN(this.phoneNum[0])){
        this.phoneNum = this.phoneNum.slice(1);
      }
      this.v$.$validate()
      window.scrollTo(0,0);
       if(!this.v$.$invalid){
         this.isSubmited = true;
         if(this.isSubmited === this.isWrong){
         this.isWrong = false;
         }
         
         console.log(`LastName:${this.lastName}, firstName:${this.firstName} date:${this.birthDate} passDate:${this.passDate}`);
        }else{
          const errorFields = this.v$.$errors;
          const labels = document.querySelectorAll('label');
          errorFields.forEach(field => {
            labels.forEach(label => {
              if(label.htmlFor === field.$property && !this.fieldsToShow.includes(label.innerText.slice(0,label.innerText.length-1))){
                label.nextElementSibling.classList.toggle('error-input')
                this.fieldsToShow.push(label.innerText.slice(0,label.innerText.length-2));
              }
            })
            this.isWrong = true;
            if(this.isSubmited === this.isWrong){
              this.isSubmited = false;
            }
          })
          
        }
    }
  }
}
</script>

<style>
@import "css/style.css";


</style>
