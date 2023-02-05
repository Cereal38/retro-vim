<script setup lang="ts">

    import { ref } from "vue";


    // Current cursor position (x : horizontal / y : vertical)
    const cursor = ref({
        x: 0,
        y: 0,
    })

    // Lines of code
    const string = ref([""]);

    // Handle char inputs
    window.addEventListener("keypress", function(e) {

        string.value[cursor.value.y] = string.value[cursor.value.y] + String.fromCharCode(e.keyCode);

    }.bind(this));

    // Handle inputs as Escape, Backspace etc...
    window.addEventListener("keydown", function(e) {

        switch (e.key) {

            // Delete last char
            // If the line is empty, delete the line (Except for the first one)
            case "Backspace":
                if (string.value[cursor.value.y].length === 1 && string.value.length > 1) {
                    string.value.splice(cursor.value.y, 1);
                    cursor.value.y--;
                } else {
                    string.value[cursor.value.y] = string.value[cursor.value.y].slice(0, -1);
                }
                break;
            
            // Create a new line and put cursor on it
            case "Enter":
                string.value.push("");
                cursor.value.y++;
                break;
        }
    }.bind(this));

    // String that take use input


</script>


<template>
    <div class="code-space">

        <ul class="code-space__lines">

            <li 
                class="code-space__lines__line" 
                v-for="(line, index) in string" 
                :key="index"
            >
                {{ index + 1  }} : {{ line }}
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