<script setup lang="ts">

    import { ref } from "vue";


    const string = ref("Input : ");

    // Handle char inputs
    window.addEventListener("keypress", function(e) {

        string.value = string.value + String.fromCharCode(e.keyCode);
        
    }.bind(this));

    // Handle inputs as Escape, Backspace etc...
    window.addEventListener("keydown", function(e) {

        switch (e.key) {

            // Delete last char
            case "Backspace":
                string.value = string.value.slice(0, -1);
                break;
        }
        console.log(e);
    }.bind(this));

    // String that take use input


</script>


<template>
    <div class="code-space__main">

        <span>{{ string }}</span>

    </div>
</template>


<style scoped>

    .code-space__main {
        display: flex;
        height: 100%;
        width: 100%;
        flex-direction: column;
        gap: 20px;
    }

</style>