<template>
    <main>
        <div class="container text-center">
            <h1>{{ gwapo }} How To Install and Configure Vue 3 App in Laravel 10 with Vite 3 - W3TechPoint</h1>
            <blockquote class="text-center h5">Hi friends, in this tutorial you have learnt that how to install and configure a basic Vue 3 application in Laravel 10 using Vite 3.<br>To learn more, please visit our <strong><a href="https://www.laravelclick.com/category/laravel">Recommended Laravel Tutorials</a></strong> and improvise yourself.</blockquote>
            <button v-if="trigger" @click="gwapoko()" >Hi</button>
        </div>

        <div id="app">
            <button @click="increment">
                Count is: {{ count }}
            </button>
        </div>

        <div>
            <label class="file-select">
                
                <div class="select-button">
                    <span v-if="value">Selected File: {{value.name}}</span>
                    <span v-else>Select File</span>
                </div>
                
                <input type="file" @change="handleFileChange"/>
            </label>
        </div>

        
        
    </main>
</template>

<script setup>
import { ref, onMounted  } from "vue";

const gwapo = ref('ako')
const trigger = ref(true)

const count = ref(0)

function increment() {
    count.value++
}

onMounted(() => {
    gwapo.value = 'ikaw'
    console.log('The initial count is', count.value)
})

const gwapoko = () =>{
    alert(gwapo.value)
}



</script>

<style scoped>
    button {
        font-weight: bold;
        font-size: larger;
    }
</style>