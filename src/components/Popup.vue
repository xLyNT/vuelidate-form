<template>
    <div class ="popup" :class="{ 'popup-active' : isActive}">
        <h3 :style="{backgroundColor:color}">{{heading}}</h3>
        <p v-if="color !== 'green'"><span :key="field" v-for="field of fieldsToShow">{{fieldsToShow.indexOf(field) === fieldsToShow.length - 1 ? field + '.' : field + ', ' }}</span></p>
    </div>
</template>

<script>

export default ({
    name:'Popup',
    props:{
        heading:String,
        message:String,
        isActive:Boolean,
        color:String,
        fieldsToShow:Object
    }
})
</script>
<style>
@import "../css/style.css";

</style>
