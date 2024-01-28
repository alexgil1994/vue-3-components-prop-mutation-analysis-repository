<script setup lang="ts">
import { ref, computed, watch, onMounted } from "vue";
import TestComp from "../components/TestComp.vue";
import TestObjCompByReferenceWithLocalGetSet from "../components/TestObjCompByReferenceWithLocalGetSet.vue";
import TestObjCompByReference from "../components/TestObjCompByReference.vue";
import TestObjCompByReferenceAsProp from "../components/TestObjCompByReferenceAsProp.vue";
import { CustomClass } from "utils/Models";

const msg = ref("Hello World!");

const nameTest = ref("");

const nameObj = ref({ name: "", surname: 0 });

//
const nameFromObjComputed = computed(() => nameObj.value.name);

watch(nameFromObjComputed, (newValue) => {
  console.log("From Parent: new object name value" + newValue);
  // Both work normally (setting another property based on the value, setting a new object keeping the new value and changing another property)
  // nameObj.value.surname = value.length;
  nameObj.value = { name: newValue, surname: newValue.length }; // Careful here, if we were to change also the name property we would end up obviously in a recursive error
});

//
const testCustomClass = ref<CustomClass>({
  name: "",
  surname: "",
} as CustomClass);

const nameFromTestCustomClassComputed = computed(
  () => testCustomClass.value.name
);

// -Works when by either of the by reference components
watch(nameFromTestCustomClassComputed, (value) => {
  console.log("From Parent-watcher: new test object name value" + value);
  testCustomClass.value.surname = "new test value from parent";
});

// This works and the inner input value of the component is updated and an inner watcher can be triggered
onMounted(() => {
  setTimeout(() => {
    console.log("From Parent-mounted: timeout called");
    // testCustomClass.value.name = "test value from timeout";
    // --- This works only with the get set local component because the getter is called on object change and initially
    testCustomClass.value = {
      name: "testing name 1",
      surname: "testing surname 1",
    };
  }, 10000);
});
</script>

<template>
  Standard input <br />
  <h1>{{ msg }}</h1>
  <input v-model="msg" />

  <div style="margin-top: 20px">
    Test Comp through ref <br />
    {{ nameTest }}
    <test-comp v-model="nameTest" />
  </div>

  <div style="margin-top: 20px">
    Test Comp through object's passed property <br />
    {{ nameObj.name }}
    <test-comp v-model="nameObj.name" />

    {{ nameObj.surname }}
  </div>

  <div style="margin-top: 20px">
    Test Obj Comp By reference get/set <br />
    {{ testCustomClass.name }}
    <test-obj-comp-by-reference-with-local-get-set v-model="testCustomClass" />

    {{ testCustomClass.surname }}
  </div>

  <div style="margin-top: 20px">
    Test Obj Comp By reference <br />
    {{ testCustomClass.name }}
    <test-obj-comp-by-reference v-model="testCustomClass" />

    {{ testCustomClass.surname }}
  </div>

  <div style="margin-top: 20px">
    Test Obj Comp By reference as prop <br />
    {{ testCustomClass.name }}
    <test-obj-comp-by-reference-as-prop :dto="testCustomClass" />

    {{ testCustomClass.surname }}
  </div>
</template>
