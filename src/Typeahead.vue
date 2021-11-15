<template>
  <div class="typeahead-container">
    <div class="search-container">
      <input
        v-model="searchTerm"
        type="search"
        :placeholder="props.placeholder"
        :class="{ 'has-results': results.length }"
        @search="handleClear"
        @keyup.up="handleArrow(-1)"
        @keyup.down="handleArrow(1)"
        @keyup.esc="handleEsc"
        @keyup.enter="handleSelect"
      />
    </div>
    <div class="results-container">
      <ul v-if="results.length && !selected" class="results">
        <template v-for="(result, index) in results" :key="index">
          <li
            v-if="index < props.maxResults"
            :class="{ focused: isFocused(index) }"
            @click="select(result)"
            @mouseover="focus(index)"
            @mouseleave="focus(-1)"
          >
            <h5 class="category">
              {{ result[categoryKey] }}
            </h5>
            {{ result[valueKey] }}
          </li>
          <li v-if="results.length === 0">Test</li>
        </template>
      </ul>
      <ul
        v-else-if="searchTerm && !results.length && !selected"
        class="results"
      >
        <li>No results found!</li>
      </ul>
    </div>
  </div>
</template>

<script lang="ts" setup>
import { ref, toRefs, ComputedRef, computed, watch } from "vue";

interface Props {
  modelValue: string;
  suggestions: Record<string, string>[];
  placeholder?: string;
  searchKey?: string;
  categoryKey?: string;
  valueKey?: string;
  maxResults?: number;
}

const props = withDefaults(defineProps<Props>(), {
  placeholder: "Type to search...",
  searchKey: "value",
  categoryKey: "category",
  valueKey: "value",
  maxResults: 5,
});

const emit = defineEmits<{
  (e: "update:modelValue", result?: Record<string, string>): void;
}>();

const searchTerm = ref("");
const selected = ref(false);
const focused = ref(-1);
const { valueKey, suggestions, searchKey, modelValue } = toRefs(props);

const results: ComputedRef<Record<string, string>[]> = computed(
  (): Record<string, string>[] => {
    if (!searchTerm.value?.length || selected.value) return [];
    return suggestions.value.filter((entry: Record<string, string>) =>
      entry[searchKey.value]
        .toLowerCase()
        .includes(searchTerm.value.toLowerCase())
    );
  }
);

const select = (result: Record<string, string>): void => {
  searchTerm.value = result[valueKey.value];
  selected.value = true;
  return emit("update:modelValue", result);
};

const handleArrow = (dir: number): void => {
  if (dir < 0) {
    if (focused.value > 0) {
      focused.value--;
    }
  } else if (dir > 0 && focused.value < results.value.length - 1) {
    focused.value++;
  }
};

const handleEsc = (): void => {
  searchTerm.value = "";
  selected.value = false;
  focused.value = -1;
  return emit("update:modelValue", undefined);
};

const handleClear = (): void => {
  if (!searchTerm.value) {
    handleEsc();
  }
};

const handleSelect = (): void => {
  const result = results.value[focused.value];
  select(result);
};

const focus = (index: number): void => {
  focused.value = index;
};

const isFocused = (index: number): boolean => {
  return index === focused.value;
};

watch(modelValue, (newVal: string, oldVal: string) => {
  if (newVal !== oldVal && newVal !== searchTerm.value) {
    searchTerm.value = newVal;
  }
});

watch(searchTerm, (newVal: string) => {
  if (!newVal || newVal.length === 0) {
    selected.value = false;
  }
});
</script>

<style scoped lang="scss">
* {
  box-sizing: border-box;
}

html,
body {
  margin: 0;
}

body {
  color: #333;
  font-family: sans-serif;
  line-height: 1.7;
  background-color: skyblue;
}

.container {
  display: flex;
  width: 300px;
  margin: 2.5em auto;
  justify-content: flex-start;
  #app {
    width: 100%;
  }
}

label {
  font-weight: bold;
}
.category {
  font-size: 14px;
  font-weight: 550;
  text-align: left;
  margin: 2px 0;
}

.typeahead-container {
  position: relative;
  .search-container {
    input {
      width: 100%;
      padding: 8px 10px;
      color: #333;
      border: 2px solid darken(#f2f2f2, 5%);
      border-radius: 5px;
      &:focus {
        outline: none;
      }
    }
  }
  .results-container {
    position: absolute;
    top: 105%;
    width: 100%;
  }
  .results {
    font-size: 16px;
    text-align: left;
    display: block;
    width: 100%;
    margin: 0;
    padding: 0;
    list-style-type: none;
    background-color: #fff;
    border: 1px solid #f2f2f2;
    border-radius: 5px;
    li {
      padding: 2px 6px;
      color: #333;
      &:hover {
        cursor: pointer;
      }
      &.focused,
      &:hover {
        color: #fff;
        background: #0087ff;
        &:first-child {
          border-top-right-radius: 5px;
          border-top-left-radius: 5px;
        }
        &:last-child {
          border-bottom-right-radius: 5px;
          border-bottom-left-radius: 5px;
        }
      }
      &:not(:last-child) {
        border-bottom: 1px solid #f2f2f2;
      }
    }
  }
}
</style>
