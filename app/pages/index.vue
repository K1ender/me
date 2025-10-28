<script setup lang="ts">
const hueRotate = ref(123);

onMounted(() => {
    const interval = setInterval(() => {
        hueRotate.value += 1;

        if (hueRotate.value >= 360) {
            hueRotate.value = 0;
        }
    }, 100);

    onBeforeUnmount(() => {
        clearInterval(interval);
    });
})
</script>

<template>
    <div class="h-screen flex items-center justify-center">
        <h1 class="h-auto text-center text-[15vw] whitespace-normal rubik-glitch-pop">
            Welcome to <i :style="`color: hsl(${hueRotate}deg 100% 50%)`">me</i>
        </h1>
    </div>

    <div class="h-screen rubik flex w-full flex-col items-center">
        <h2 class="text-[10vw]">About me</h2>
        <div class="h-full w-full flex items-center justify-center">
            <ul class="h-full flex flex-col justify-around text-center text-[4vw]">
                <li data-cursor class="hover-effect">Web apps.</li>
                <li data-cursor class="hover-effect">Data. Containers.</li>
                <li data-cursor class="hover-effect">Ideas turned into code.</li>
            </ul>
        </div>
    </div>
</template>

<style>
.hover-effect {
    position: relative;
    display: inline-block;
    transition: transform 0.5s ease, color 0.5s ease;
    color: #111;
}

.hover-effect::before {
    content: '';
    position: absolute;
    width: 0%;
    height: 3px;
    bottom: -5px;
    left: 50%;
    background-color: #3b82f6;
    transition: 0.5s ease;
    transform: translateX(-50%);
}

.hover-effect:hover {
    color: #3b82f6;
    transform: scale(1.15) rotate(-2deg);
}

.hover-effect:hover::before {
    width: 80%;
}
</style>