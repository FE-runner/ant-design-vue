<style scoped lang="scss">
.BSelect {
}
</style>
<template>
  <Select
    v-bind="selectProps"
    v-model:value="compValue"
    class="BSelect"
    @change="handleChange"
  ></Select>
</template>
<script setup lang="tsx">
import { ref, watch } from 'vue';
import type { SelectProps, SelectValue } from '../select';
import Select from '../select';

const Props = defineProps<{
  selected: SelectValue;
  selectProps: SelectProps;
}>();
const Emits = defineEmits(['update:selected']);
const compValue = ref<SelectValue>(undefined);
watch(
  () => Props.selected,
  newValue => {
    if (newValue === '') {
      compValue.value = undefined;
    } else {
      compValue.value = Props.selected;
    }
  },
  {
    immediate: true,
    deep: true,
  },
);

const handleChange = (value: SelectValue) => {
  Emits('update:selected', value === undefined ? '' : value);
};
</script>
<script lang="tsx">
import { defineComponent } from 'vue';
export default defineComponent({
  name: 'BSelect',
});
</script>
