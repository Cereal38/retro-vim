<script setup lang="ts">
import CodeSpace from "@/components/CodeSpace.vue"
</script>


<template>
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
    color: #12da33dc;
}

</style>