<template>
  <input v-model="localModelValue.name" type="text" />
</template>

<script setup lang="ts">
import { computed, ref, watch } from "vue";

import { CustomClass } from "utils/Models";

const props = defineProps<{
  modelValue: CustomClass;
}>();

const localModelValue = ref(props.modelValue);

const localinput = computed(() => localModelValue.value.name);

// Works-triggered both from inside and from outside
watch(localinput, (value: string) => {
  console.log("inner by reference (WITHOUT local get set) called");
  console.log(value);
  console.log(localModelValue.value.surname);

  localModelValue.value.name = value;
  localModelValue.value.surname = value + "test surname";
});
</script>
