<script setup lang="ts">
import CurrentTabHandler from "./components/CurrentTabHandler.vue";
import { useEventListener } from "@vueuse/core";
import { PopupAction } from "@/enums/popup-state.enum";

let openedWindow: Window | null = null;

const handleOpen = () => {
  if (!window.opener) {
    const blockWidth = 600;
    const blockHeight = 300;

    const leftOffset = window.screen.width / 2 - blockWidth / 2;
    const topOffset = window.screen.height / 2 - blockHeight / 2;

    if (openedWindow) {
      openedWindow.close();
      return;
    }

    openedWindow = window.open(
      "/popup?state=lost-focus",
      "_blank",
      `width=${blockWidth},height=${blockHeight},popup=true,left=${leftOffset},top=${topOffset}`
    );
  }
};

useEventListener(window, "message", (event) => {
  if (!Object.values(PopupAction).includes(event.data)) return;

  const popupAction = event.data as PopupAction;

  switch (popupAction) {
    case PopupAction.CLOSE:
      openedWindow?.close();
      break;
  }
});
</script>

<template>
  <router-view></router-view>
</template>

<style scoped>
.logo {
  height: 6em;
  padding: 1.5em;
  will-change: filter;
  transition: filter 300ms;
}
.logo:hover {
  filter: drop-shadow(0 0 2em #646cffaa);
}
.logo.vue:hover {
  filter: drop-shadow(0 0 2em #42b883aa);
}
</style>
