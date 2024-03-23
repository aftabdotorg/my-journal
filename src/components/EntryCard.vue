<script lang="ts" setup>
import Entry from "@/types/Entry";
import DateDisplay from "./DateDisplay.vue";
import UseEmojis from "@/composables/UseEmojis";
import { userInjectionKey } from "@/injectionKeys";
import { inject } from "vue";
const { findEmoji } = UseEmojis();
const injectedUser = inject(userInjectionKey)

interface Props {
  entry: Entry
}

const props = defineProps<Props>()
const { entry } = props
</script>
<template>
  <div class="entry-card">
    <div class="entry-card-body">
      <component width="75" :is="findEmoji(entry.emoji)"></component>
      <div class="entry-text">{{ entry.body }}</div>
    </div>
    <div class="entry-footer">
      <DateDisplay :date="new Date()" class="mr-2" />
      |
      <span class="ml-2">{{ injectedUser?.username || "anonymous" }}</span>
    </div>
  </div>
</template>
