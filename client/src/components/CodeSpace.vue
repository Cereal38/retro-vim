<script setup lang="ts">

    import { ref } from "vue";

    const initialRowLen = 5;
    const maxRowLen = 80;

    // Lines of code
    const lines = ref(["\r"]);

    // Current cursor position (x : horizontal / y : vertical)
    const cursor = ref({
        x: initialRowLen,
        y: 0,
    })

	// States of the automata
	const INIT = "INIT";
	const INSERT = "INSERT";
	const NUM = "NUM";
	
	// Alphabet of the automata
	const I = "i";
	const ESC = "Escape";

	// Transition table
	const transitions = (input) => {
		
		switch (automata.value) {

			case INIT:
				switch (input) {
					case I:
						automata.value = INSERT; break;
					case ESC:
						automata.value = INIT; break;
					case "0":
					case "1":
					case "2":
					case "3":
					case "4":
					case "5":
					case "6":
					case "7":
					case "8":
					case "9":
						automata.value = NUM;
						break;
					default:
						console.log("Error (Input unknown)");
						automata.value = INIT; break;
				}
				break;

			case INSERT:
				switch (input) {
					case ESC:
						automata.value = INIT; break;
					default:
						// Write the input in the keypress event listener
						break;
				}
				break;

			case NUM:
				switch (input) {
					case ESC:
						automata.value = INIT; break;
					case "0":
					case "0":
					case "1":
					case "2":
					case "3":
					case "4":
					case "5":
					case "6":
					case "7":
					case "8":
					case "9":
					case "1":
					case "2":
					case "3":
					case "4":
					case "5":
					case "6":
					case "7":
					case "8":
					case "9":
						break;
					default:
						break;
				}

			default:
				console.log("ERROR (Current state unknown) :", automata.value);
				return INIT;
		}
	}

	const automata = ref(INIT);

    // Handle inputs
    window.addEventListener("keydown", function(e) {

        console.log("KEY :", e.key)

		transitions(e.key);

		console.log("AUTOMATA :", automata.value);
		
    }.bind(this));

    window.addEventListener("keypress", function(e) {

		// If insert mode add the char to the char
		if (automata.value == INSERT) {

			// Except
			if (e.key === "Enter") { return }

			lines.value[cursor.value.y] += e.key;

			// Move the cursor to the right
			cursor.value.x++;
		}
		
    }.bind(this));


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
            v-if="automata === 'INSERT'"
            :style="{
                gridColumn: cursor.x + 1,
                gridRow: cursor.y + 1,
            }"
        />
        <div 
            class="code-space__cursor-normal"
            v-if="automata === 'INIT'"
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
        background-color: #52fa33;
    }

    .code-space__cursor-normal {
        height: 20px;
        width: 20px;
        background-color: #52fa33;
    }
    
</style>
