<template>
  <div
    style="margin: 8px"
    :class="{ focused: inputFocused, filled: inputData.length > 0 }"
  >
    <div class="dropdown" :class="{ focused: inputFocused }">
      <div class="input-outer-box">
        <div class="input-group">
          <label class="input-label" :for="'text-input-' + localId"
            >Врач, заболевания, услуги</label
          >
          <input
            ref="inputRef"
            v-model="inputData"
            :id="'text-input-' + localId"
            type="text"
            @keydown="onInputKeyDown"
            @keyup.enter="onInputKeyDownEnter"
            @focus="inputFocused = true"
            @blur="inputFocused = false"
          />
        </div>
        <div style="display: flex">
          <div
            style="margin-right: 8px; cursor: pointer"
            @click="inputData = ''"
          >
            <span class="material-icons-outlined"> close </span>
          </div>
          <div>
            <span class="material-icons-outlined expand-icon">
              expand_more
            </span>
          </div>
        </div>
      </div>
      <div v-if="inputFocused" class="dropdown-content">
        <p style="padding: 4px 14px; color: #808080">Специальность</p>
        <hr style="border-top: 1px solid #9cbdff" />
        <div class="dropdown-list-content">
          <div
            v-if="dataView.length > 0"
            class="list-item"
            v-for="item in dataView"
            :key="item.id"
            @mousedown="onListClick(item)"
          >
            {{ item.title }}
          </div>
          <div style="padding: 8px 14px" v-else>Не найдено специальностей</div>
        </div>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
let id = 0;
</script>

<script setup lang="ts">
import { computed } from "@vue/reactivity";
import { ref } from "vue";

export interface SelectDataType {
  title: string;
  id: string;
  url: string;
  type: string;
}

const props = withDefaults(
  defineProps<{
    initialData?: SelectDataType[];
  }>(),
  {
    initialData: () => [],
  }
);

const localId = id++;
const inputRef = ref<HTMLInputElement>();
const onListClick = (item: any) => {
  inputData.value = item.title;
};
const onInputKeyDown = () => {};
const onInputKeyDownEnter = () => {
  if (dataView.value.length > 0 && inputData.value.length > 0) {
    inputData.value = dataView.value[0].title;
  }
};
const inputFocused = ref(false);
const inputData = ref("");

const dataView = computed<SelectDataType[]>(() => {
  if (inputData.value.length === 0) {
    return props.initialData;
  } else
    return props.initialData.filter((el) => {
      return el.title.toLowerCase().startsWith(inputData.value.toLowerCase());
    });
});
</script>

<style scoped>
.dropdown-list-content {
  max-height: 300px;
  overflow-y: scroll;
}

::-webkit-scrollbar-track {
  background: #f1f4f9;
}

::-webkit-scrollbar {
  width: 5px;
}

/* Handle */
::-webkit-scrollbar-thumb {
  background: #387aff;
  border-radius: 10px;
}

p {
  margin: 0 !important;
}

.list-item {
  padding: 4px 14px;
  cursor: pointer;
}

.list-item:hover {
  background-color: #f2f8ff;
}

.dropdown {
  position: relative;
}

.dropdown-content {
  display: none;
  position: absolute;
  margin-top: 8px;
  background-color: #fff;
  border-radius: 5px;
  box-shadow: 0px 14px 24px rgba(31, 49, 73, 0.25);
  z-index: 100;
  width: 100%;
}

.dropdown:hover .dropdown-content {
  display: block;
}

.focused .dropdown-content {
  display: block;
}

.input-outer-box {
  border-radius: 10px;
  height: 54px;
  background-color: #f2f8ff;
  padding-left: 20px;
  padding-right: 20px;
  box-sizing: border-box;
  border: 1px solid transparent;
  transition: border 0.1s ease-in;
  display: flex;
  align-items: center;
  justify-content: space-between;
}

.input-outer-box:hover,
.focused .input-outer-box {
  border: 1px solid blue;
}

input {
  position: relative;
  width: 100%;
  outline: 0;
  height: 100%;
  outline: none;
  padding-top: 10px;
  border: none;
  background-color: transparent;
  font-size: 1rem;
}

.input-group {
  position: relative;
  height: 100%;
  flex-grow: 1;
}

.input-label {
  cursor: text;
  position: absolute;
  width: 300px;
  top: 15px;
  left: 0;
  color: #999;
  background-color: transparent;
  z-index: 10;
  transition: transform 0.15s ease-in, font-size 0.15s ease-in;
}

.focused .input-label,
.filled .input-label {
  transform: translateY(-70%);
  font-size: 0.75em;
}

.expand-icon {
  transition: transform 0.15s ease-in-out;
}

.focused .expand-icon,
.filled .expand-icon {
  transform: rotate(180deg);
}

.material-icons-outlined {
  color: #757f93;
}
</style>
