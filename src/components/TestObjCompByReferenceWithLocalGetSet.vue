<template>
  <input v-model="localModel.name" type="text" />
</template>

<script setup lang="ts">
import { computed, ref, watch } from "vue";

import { CustomClass } from "utils/Models";

const props = defineProps<{
  modelValue: CustomClass;
}>();

const emits = defineEmits(["update:modelValue"]);

const localModel = computed({
  get: () => {
    console.log("getter from get set comp called");

    return props.modelValue;
  },
  set: (value: CustomClass) => {
    console.log("firing setter event with value" + value);
    emits("update:modelValue", value);
  },
});

// Still works without emit
// const localModel = computed({
//   get: () => {
//     console.log("getter");

//     return props.modelValue;
//   },
//   set: (value: CustomClass) => {
//     console.log("firing event with value" + value);
//     // emits("update:modelValue", value);
//   },
// });

const localinput = computed(() => localModel.value.name);

// Works-triggered both from inside and from outside
watch(localinput, (value: string) => {
  console.log("inner by reference (WITH local get set) called");
  console.log(value);
  console.log(localModel.value.surname);

  localModel.value.name = value;
  localModel.value.surname = value + "test surname";
});
</script>
