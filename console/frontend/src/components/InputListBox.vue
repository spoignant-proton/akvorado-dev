<template>
  <Listbox
    :multiple="multiple"
    :model-value="modelValue"
    @update:model-value="(item) => $emit('update:modelValue', item)"
  >
    <div class="relative">
      <InputComponent v-slot="{ id, childClass }" v-bind="$attrs">
        <ListboxButton :id="id" :class="childClass">
          <span class="block truncate pr-10 text-left">
            <slot name="selected"></slot>
          </span>
          <span
            class="pointer-events-none absolute inset-y-0 right-0 flex items-center pr-2"
          >
            <SelectorIcon class="h-5 w-5 text-gray-400" aria-hidden="true" />
          </span>
        </ListboxButton>
      </InputComponent>

      <transition
        leave-active-class="transition duration-100 ease-in"
        leave-from-class="opacity-100"
        leave-to-class="opacity-0"
        class="z-10 rounded bg-white shadow dark:bg-gray-700"
      >
        <ListboxOptions
          class="absolute max-h-60 w-full overflow-auto py-1 text-sm text-gray-700 dark:text-gray-200"
        >
          <ListboxOption
            v-for="item in items"
            v-slot="{ selected, active }"
            :key="item.id"
            :value="item"
            as="template"
          >
            <li
              class="relative inline-flex w-full cursor-default select-none py-2 pl-10 pr-4 text-sm hover:bg-gray-100 dark:hover:bg-gray-600 dark:hover:text-white"
              :class="{
                'bg-gray-100 dark:bg-gray-600 dark:bg-gray-600 dark:text-white':
                  active,
              }"
            >
              <span class="block truncate">
                <slot name="item" v-bind="item"></slot>
              </span>
              <span
                v-if="selected"
                class="absolute inset-y-0 left-0 flex items-center pl-3 text-blue-600 dark:text-blue-500"
              >
                <CheckIcon class="h-5 w-5" aria-hidden="true" />
              </span>
            </li>
          </ListboxOption>
        </ListboxOptions>
      </transition>
    </div>
  </Listbox>
</template>

<script setup>
defineProps({
  modelValue: {
    type: Object,
    required: true,
  },
  items: {
    // Each item in the array is expected to have "id" and "name".
    type: Array,
    required: true,
  },
  multiple: {
    type: Boolean,
    default: false,
  },
});
defineEmits(["update:modelValue"]);

import {
  Listbox,
  ListboxButton,
  ListboxOptions,
  ListboxOption,
} from "@headlessui/vue";
import { CheckIcon, SelectorIcon } from "@heroicons/vue/solid";
import InputComponent from "@/components/InputComponent.vue";
</script>