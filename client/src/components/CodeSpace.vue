<script setup lang="ts">

    import { ref } from "vue";


    const string = ref("Input : ");

    // Detect user inputs and add it to the string
    window.addEventListener("keypress", function(e) {
        string.value = string.value + String.fromCharCode(e.keyCode);
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