<script lang="ts" setup>
import EmojiField from "@/components/EmojiField.vue";
import ArrowCircleRight from "@/assets/icons/arrow-circle-right.svg";
import { userInjectionKey } from "@/injectionKeys";
import type Emoji from "@/types/Emoji";
import type Entry from "@/types/Entry";
import { ref, computed, onMounted, inject } from "vue"
const injectedUser = inject(userInjectionKey)


// ! events
const emit = defineEmits<{
  (e: "@create", entry: Entry): void
}>()

// template refs to auto focus on mount
const textarea = ref<HTMLTextAreaElement | null>(null)
onMounted(() => textarea.value?.focus())

const body = ref("");
const emoji = ref<Emoji | null>(null)
const charCount = computed(() => body.value.length)
const maxCharLimit = 280

const handleTextInput = (e: Event) => {
  const textArea = e.target as HTMLTextAreaElement
  if (textArea.value.length <= maxCharLimit) {
    body.value = textArea.value
  } else {
    body.value = textArea.value = textArea.value.substring(0, maxCharLimit)
  }
}

const handleSubmit = () => {
  emit('@create', { body: body.value, emoji: emoji.value, userId: 1, id: Math.random(), createdAt: new Date() })
  body.value = ""
  emoji.value = null
}
</script>

<template>
  <form class="entry-form" @submit.prevent="handleSubmit">
    <textarea v-model="body" ref="textarea" @keyup="handleTextInput"
      :placeholder="`New Journal Entry for ${injectedUser?.username || 'anonymous'}`"></textarea>
    <EmojiField v-model="emoji" />
    <div class="entry-form-footer">
      <span>{{ charCount }} / {{ maxCharLimit }}</span>
      <button>Remember
        <ArrowCircleRight width="20" />
      </button>
    </div>
  </form>
</template>
