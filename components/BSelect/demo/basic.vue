<docs>
---
order: 0
title:
  zh-CN: 基本
  en-US: Basic
---

## zh-CN

展示文档样例。

## en-US

Display the document sample.
</docs>
<template>
  <div class="">
    <div>单选</div>
    <div>
      <b-select
        style="width: 400px"
        v-model:selected="value"
        :selectProps="{
          allowClear: true,
          showSearch: true,
          placeholder: '请选择',
          options: options,
        }"
      ></b-select>
    </div>
    <div>多选</div>
    <div>
      <BSelect
        style="width: 400px"
        v-model:selected="value2"
        :selectProps="{
          mode: 'multiple',
          allowClear: true,
          showSearch: true,
          placeholder: '请选择',
          options: options,
        }"
      ></BSelect>
    </div>
  </div>
</template>
<script lang="ts" setup>
import { ref } from 'vue';

const value = ref<string>('');
const value2 = ref<string[]>([]);
const options = [
  { label: '选项1', value: '1' },
  { label: '选项2', value: '2' },
  { label: '选项3', value: '3' },
];
</script>
