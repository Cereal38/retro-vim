<script setup lang="ts">
import CodeSpace from "@/components/CodeSpace.vue"
</script>


<template>
    <div class="layout-terminal__main">

        <CodeSpace />

    </div>
</template>


<style scoped>

@import url('https://fonts.googleapis.com/css2?family=Press+Start+2P&display=swap');

.layout-terminal__main {
    display: flex;
    padding: 20px;
    height: calc(100vh - 40px);
    width: calc(100vw - 40px);
    font-family: "Press Start 2P", cursive;
    background-color: #0A000A;
    color: #55AA00;
}

.layout-terminal__main__code-space {
    display: flex;
    flex-direction: column;
    gap: 20px;
}
</style>