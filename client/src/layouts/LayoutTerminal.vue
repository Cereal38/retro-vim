<script setup lang="ts">
	import CodeSpace from "@/components/CodeSpace.vue"
</script>


<template>

	<div class="layout-terminal__layer" />

    <div class="layout-terminal">
			<CodeSpace />
    </div>

</template>


<style scoped>

@import url('https://fonts.googleapis.com/css2?family=Press+Start+2P&display=swap');

.layout-terminal {
    display: flex;
    padding: 20px;
    height: calc(100vh - 40px);
    width: calc(100vw - 40px);
    font-family: "Press Start 2P", cursive;
    background-color: #1d1d1d;
    color: #52fa33;
}

.layout-terminal__layer {
	position: fixed;
	width: 100vw;
	height: 100vh;
	filter: brightness(0.7) contrast(0.8);
	background-color: #00a500;
	opacity: 0.5;
	box-shadow:inset 0px 0px 50vh 150px #002500;
}

</style>
