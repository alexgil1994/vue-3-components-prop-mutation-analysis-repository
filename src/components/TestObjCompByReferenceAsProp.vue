<template>
  <input v-model="localModelValue.name" type="text" />
</template>

<script setup lang="ts">
import { computed, ref, watch, onMounted } from "vue";

import { CustomClass } from "utils/Models";

const props = defineProps<{
  dto: CustomClass; // It can also be a prop
}>();

const localModelValue = ref(props.dto);

const localinput = computed(() => localModelValue.value.name);

// Works-triggered both from inside and from outside
watch(localinput, (value: string) => {
  console.log("inner by reference (WITHOUT local get set) called");
  console.log(value);
  console.log(localModelValue.value.surname);

  localModelValue.value.name = value;
  localModelValue.value.surname = value + "test surname";
});

// --- This doesnt update the parent and the other components, new object loses reference for other connections
// onMounted(() => {
//   setTimeout(() => {
//     localModelValue.value = {
//       name: "testing name 1",
//       surname: "testing surname 1",
//     };
//   }, 20000);
// });
</script>
