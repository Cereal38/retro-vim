<script setup lang="ts">

    import { ref } from "vue";

    // Current cursor position (x : horizontal / y : vertical)
    const cursor = ref({
        x: 0,
        y: 0,
    })

    // Lines of code
    const lines = ref([""]);

    // Handle char inputs
    window.addEventListener("keypress", function(e) {

        // 80 char max per line
        if (lines.value[cursor.value.y].length > 79) { return }

        lines.value[cursor.value.y] = lines.value[cursor.value.y] + String.fromCharCode(e.keyCode);

        if (e.key !== "Enter") {
            cursor.value.x++;
        }
        
    }.bind(this));

    // Handle inputs as Escape, Backspace etc...
    window.addEventListener("keydown", function(e) {

        switch (e.key) {

            // Delete last char
            // If the line is empty, delete the line (Except for the first one)
            case "Backspace":
                if (lines.value[cursor.value.y].length === 1 && lines.value.length > 1) {
                    lines.value.splice(cursor.value.y, 1);
                    cursor.value.y--;
                    cursor.value.x = lines.value[cursor.value.y].length-1;
                } else {
                    lines.value[cursor.value.y] = lines.value[cursor.value.y].slice(0, -1);
                    cursor.value.x--;
                }
                break;
            
            // Create a new line and put cursor on it
            case "Enter":
                lines.value.push("");
                cursor.value.y++;
                cursor.value.x = 0;
                break;
            
            // Add a tab
            case "Tab":
                event?.preventDefault(); // Remove the normal behavior of the tab key
                lines.value[cursor.value.y] = lines.value[cursor.value.y] + '    ';
                break;

            default:
                break;
        }
        
        // DEBUG
        console.log(lines.value)

    }.bind(this));

    // lines that take use input


</script>


<template>
    <div class="code-space">
        <div 
            class="code-space__lines__line" 
            v-for="(line, indexLine) in lines" 
            :key="indexLine"
            :style="{
                gridColumn: 1,
                gridRow: indexLine + 1,
            }"
        >
            <span 
                class="code-space__lines__line__content" 
                v-for="(char, indexChar) in line"
                :key="indexChar"
            >
                <span 
                    v-html="char.replace(/ /g, '&nbsp;')"
                ></span>
            </span>
        </div>

        <!-- Cursor -->
        <div 
            class="code-space__cursor"
            :style="{
                gridColumn: cursor.x + 1,
                gridRow: cursor.y + 1,
            }"
        />

    </div>
</template>


<style scoped>

    .code-space {
        display: grid;
        grid-template-columns: repeat(80, 20px);
        row-gap: 4px;
        grid-auto-rows: 20px;
        height: 100%;
        width: 100%;
        font-size: 20px;
    }

    .code-space__cursor {
        height: 20px;
        width: 20px;
        background-color: aliceblue;
    }
    
</style>