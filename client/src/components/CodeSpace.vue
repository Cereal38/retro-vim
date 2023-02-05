<script setup lang="ts">

    import { ref } from "vue";

    const initialRowLen = 5;
    const maxRowLen = 80;

    const mode = ref("normal");

    // Current cursor position (x : horizontal / y : vertical)
    const cursor = ref({
        x: initialRowLen,
        y: 0,
    })

    const checkCursorPosition = (x: number, y: number) => {
        if (x < initialRowLen || x > maxRowLen + initialRowLen || y < 0 || y > 1000) { return 0 }
        return 1;
    }

    // Lines of code
    const lines = ref(["\r"]);

    // Handle char inputs
    window.addEventListener("keypress", function(e) {

        // Only work in insert mpde
        if (mode.value !== "insert") { return }

        // 80 char max per line
        if (lines.value[cursor.value.y].length >= maxRowLen) { return }

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

                if (cursor.value.y > 0 && lines.value[cursor.value.y].length === 1) {
                    lines.value.splice(cursor.value.y, 1);
                    cursor.value.y--;
                    cursor.value.x = lines.value[cursor.value.y].length - 1 + initialRowLen;
                } else {
                    if (!checkCursorPosition(cursor.value.x - 1, cursor.value.y)) { return }
                    lines.value[cursor.value.y] = lines.value[cursor.value.y].slice(0, -1);
                    cursor.value.x--;
                }

                break;
            
            // Create a new line and put cursor on it
            case "Enter":
                lines.value.push("");
                cursor.value.y++;
                cursor.value.x = initialRowLen;
                break;
            
            // Add a tab
            case "Tab":
                event?.preventDefault(); // Remove the normal behavior of the tab key
                lines.value[cursor.value.y] = lines.value[cursor.value.y] + '    ';
                cursor.value.x += 4;
                break;
            
            // Move (if not in insert mode)
            case "h":
                if (mode.value === "insert") { return }
                if (!checkCursorPosition(cursor.value.x - 1, cursor.value.y)) { return }
                cursor.value.x--;
                break;
            case "j":
                if (mode.value === "insert") { return }
                if (!checkCursorPosition(cursor.value.x, cursor.value.y + 1)) { return }
                cursor.value.y++;
                break;
            case "k":
                if (mode.value === "insert") { return }
                if (!checkCursorPosition(cursor.value.x, cursor.value.y - 1)) { return }
                cursor.value.y--;
                break;
            case "l":
                if (mode.value === "insert") { return }
                if (!checkCursorPosition(cursor.value.x + 1, cursor.value.y)) { return }
                cursor.value.x++;
                break;
            
            // Switch in insert mode (if in normal)
            case "i":
                if (mode.value === "insert") { return }
                event?.preventDefault();
                mode.value = "insert";
                break;
            
            // Switch in normal mode
            case "Escape":
                mode.value = "normal";
                break;


            default:
                break;
        }

        console.log(e.key)

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
                :style="{
                    gridColumn: 1,
                    gridRow: indexLine + 1,
                }"
            >
                {{ indexLine + 1 }}
            </span>

            <span
                :style="{
                    gridColumn: 4,
                    gridRow: indexLine + 1,
                }"
            >
                :
            </span>

            <span 
                class="code-space__lines__line__content" 
                v-for="(char, indexChar) in line"
                :key="indexChar"
                :style="{
                    gridColumn: initialRowLen + indexChar,
                    gridRow: indexLine + 1,
                }"
            >
                <span 
                    v-html="char.replace(/ /g, '&nbsp;')"
                ></span>
            </span>
        </div>

        <!-- Cursor -->
        <div 
            class="code-space__cursor-insert"
            v-if="mode == 'insert'"
            :style="{
                gridColumn: cursor.x + 1,
                gridRow: cursor.y + 1,
            }"
        />
        <div 
            class="code-space__cursor-normal"
            v-if="mode == 'normal'"
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
        grid-template-columns: repeat(85, 20px);
        row-gap: 4px;
        grid-auto-rows: 20px;
        height: 100%;
        width: 100%;
        font-size: 20px;
    }

    .code-space__lines__line {
        display: grid;
        grid-template-columns: repeat(80, 20px);
    }

    .code-space__cursor-insert {
        transform: translateY(22px);
        height: 5px;
        width: 20px;
        background-color: aliceblue;
    }

    .code-space__cursor-normal {
        height: 20px;
        width: 20px;
        background-color: rgb(240, 248, 255, 0.7);
    }
    
</style>