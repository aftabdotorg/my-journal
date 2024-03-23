<script setup lang="ts">
import TheHeader from "@/components/TheHeader.vue";
import EntryEditor from "./components/EntryEditor.vue";
import EntryCard from "@/components/EntryCard.vue";
import { provide, reactive, inject } from "vue";
import { userInjectionKey } from "@/injectionKeys"
import type User from "./types/User";
import Entry from "./types/Entry";

const journalDB: Entry[] = reactive([])
const user: User = reactive({
  id: 1,
  username: "redstoneXf",
  settings: [],
})
provide(userInjectionKey, user)
const handleCreateEntry = (entry: Entry) => {
  journalDB.unshift(entry)
}

const injectedUser = inject(userInjectionKey)

</script>

<template>
  <main class="container m-auto p-10">
    <TheHeader />
    <EntryEditor @@create="handleCreateEntry" />
    <ul>
      <li v-for="entry in journalDB" :key="entry.id">
        <EntryCard :entry="entry" />
      </li>
    </ul>
  </main>
</template>
