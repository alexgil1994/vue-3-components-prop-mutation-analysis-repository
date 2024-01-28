<template>
  <input v-model="localModel" type="text" />
</template>

<script setup lang="ts">
import { computed } from "vue";

const props = defineProps<{
  modelValue: string;
}>();

const emits = defineEmits(["update:modelValue"]);

const localModel = computed({
  get: () => {
    return props.modelValue;
  },
  set: (value: string) => {
    console.log("firing event with value" + value);
    emits("update:modelValue", value);
  },
});
</script>
