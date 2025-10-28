<script lang="ts" setup>
import { ref, onMounted } from "vue";

const cursor = useTemplateRef("cursor")

let mouseX = 0;
let mouseY = 0;
let posX = 0;
let posY = 0;

let cursorWidth = 16;
let cursorHeight = 16;
let targetWidth = 16;
let targetHeight = 16;
let targetX = 0;
let targetY = 0;

let targetOpacity = 1;
let stuck = false;

const speed = 0.15;
const elasticity = 0.1;

function animate() {
  if (!cursor.value) return;

  if (!stuck) {
    posX += (mouseX - posX) * speed;
    posY += (mouseY - posY) * speed;
  } else {
    const dx = targetX - posX;
    const dy = targetY - posY;
    posX += dx * elasticity;
    posY += dy * elasticity;
  }

  cursorWidth += (targetWidth - cursorWidth) * speed;
  cursorHeight += (targetHeight - cursorHeight) * speed;
  cursor.value.style.width = `${cursorWidth}px`;
  cursor.value.style.height = `${cursorHeight}px`;
  cursor.value.style.opacity = `${targetOpacity}`;

  cursor.value.style.transform = `translate(${posX}px, ${posY}px) translate(-50%, -50%)`;

  requestAnimationFrame(animate);
}

onMounted(() => {
  document.addEventListener("mousemove", (e) => {
    mouseX = e.clientX;
    mouseY = e.clientY;
  });

  const interactive = document.querySelectorAll("[data-cursor]");
  interactive.forEach(el => {
    el.addEventListener("mouseenter", () => {
      const rect = el.getBoundingClientRect();
      targetWidth = rect.width;
      targetHeight = rect.height;
      targetX = rect.left + rect.width / 2;
      targetY = rect.top + rect.height / 2;
      targetOpacity = 0.5;
      stuck = true;
    });
    el.addEventListener("mouseleave", () => {
      targetWidth = 16;
      targetHeight = 16;
      targetOpacity = 1;
      stuck = false;
    });
  });

  animate();
});
</script>

<template>
  <div class="h-full w-full fixed top-0 left-0 pointer-events-none">
    <div ref="cursor" class="absolute bg-white rounded-full" style="width:16px; height:16px; opacity:1;"></div>
  </div>
</template>