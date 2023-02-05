<script setup lang="ts">

    import { ref } from "vue";


    const string = ref(["1 : ", "2 : "]);

    // Handle char inputs
    window.addEventListener("keypress", function(e) {

        string.value[0] = string.value[0] + String.fromCharCode(e.keyCode);

    }.bind(this));

    // Handle inputs as Escape, Backspace etc...
    window.addEventListener("keydown", function(e) {

        switch (e.key) {

            // Delete last char
            case "Backspace":
                string.value[0] = string.value[0].slice(0, -1);
                break;
        }
        console.log(e);
    }.bind(this));

    // String that take use input


</script>


<template>
    <div class="code-space">

        <ul class="code-space__lines">

            <li 
                class="code-space__lines__line" 
                v-for="line in string" 
                :key="line">{{ line }}
            </li>

        </ul>

    </div>
</template>


<style scoped>

    .code-space {
        height: 100%;
        width: 100%;
    }

    .code-space__lines {
        display: flex;
        flex-direction: column;
        gap: 20px;
    }

    .code-space__lines__line {
        all: unset;
    }

</style>