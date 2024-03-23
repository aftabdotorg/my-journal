<script lang="ts" setup>
import UseEmojis from "@/composables/UseEmojis";
import { userInjectionKey } from "@/injectionKeys";
import { inject } from "vue"
const { emojis } = UseEmojis();

const injectedUser = inject(userInjectionKey)
defineProps(["modelValue"]);
defineEmits(["update:modelValue"]);
</script>

<template>
  <div class="emoji-container">
    <component v-for="emoji in emojis" :is="emoji.component" :key="emoji.name"
      :class="{ selected: modelValue === emoji.name }" @click="
      $emit(
        'update:modelValue',
        emoji.name === modelValue ? null : emoji.name
      )
      "></component>
  </div>
</template>
