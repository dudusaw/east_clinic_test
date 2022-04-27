<template>
  <div :class="{ focused: inputFocused, filled: inputData.length > 0 }">
    <div class="input-outer-box">
      <div class="input-group">
        <label class="input-label" for="text-input"
          >Врач, заболевания, услуги</label
        >
        <input
          ref="inputRef"
          v-model="inputData"
          id="text-input"
          type="text"
          @keydown="onInputKeyDown"
          @keyup.enter="onInputKeyDownEnter"
          @focus="inputFocused = true"
          @blur="inputFocused = false"
        />
      </div>
      <div style="display: flex">
        <div style="margin-right: 8px; cursor: pointer" @click="inputData = ''">
          <span class="material-icons-outlined"> close </span>
        </div>
        <div>
          <span class="material-icons-outlined expand-icon"> expand_more </span>
        </div>
      </div>
    </div>
    <div class="dropdown">
      <div class="dropdown-list">
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
          <div style="padding: 4px 14px" v-else>Не найдено специальностей</div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { computed } from "@vue/reactivity";
import { ref } from "vue";

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

interface SelectDataType {
  title: string;
  id: string;
  url: string;
  type: string;
}

const dataView = computed<SelectDataType[]>(() => {
  if (inputData.value.length === 0) {
    return initialData.value;
  } else
    return initialData.value.filter((el) => {
      return el.title.toLowerCase().startsWith(inputData.value.toLowerCase());
    });
});

const initialData = ref<SelectDataType[]>([
  {
    title: "Аллерголог",
    id: "9440",
    url: "vrachi/allergolog",
    type: "spec",
  },
  {
    title: "Андролог",
    id: "11690",
    url: "vrachi/androlog",
    type: "spec",
  },
  {
    title: "Артролог",
    id: "7757",
    url: "vrachi/artrolog",
    type: "spec",
  },
  {
    title: "Вегетолог",
    id: "8385",
    url: "vrachi/vegetolog",
    type: "spec",
  },
  {
    title: "Вертебролог",
    id: "7758",
    url: "vrachi/vertebrolog",
    type: "spec",
  },
  {
    title: "Вертеброневролог",
    id: "7759",
    url: "vrachi/vertebronevrolog",
    type: "spec",
  },
  {
    title: "Висцеральный массажист",
    id: "8170",
    url: "vrachi/visczeralnyj-massazhist",
    type: "spec",
  },
  {
    title: "Висцеральный терапевт",
    id: "7760",
    url: "vrachi/visczeralnyij-terapevt",
    type: "spec",
  },
  {
    title: "Врач восточной медицины",
    id: "7763",
    url: "vrachi/vrach-vostochnoj-mediczinyi",
    type: "spec",
  },
  {
    title: "Врач китайской медицины",
    id: "7764",
    url: "vrachi/vrach-kitayskoy-mediciny",
    type: "spec",
  },
  {
    title: "Врач ЛФК",
    id: "7761",
    url: "vrachi/vrach-lfk",
    type: "spec",
  },
  {
    title: "Врач общей практики",
    id: "7765",
    url: "vrachi/vrach-obshhey-praktiki",
    type: "spec",
  },
  {
    title: "Врач первичного приёма",
    id: "7766",
    url: "vrachi/vrach-pervichnogo-priema",
    type: "spec",
  },
  {
    title: "Врач тибетской медицины",
    id: "7767",
    url: "vrachi/vrach-tibetskoy-mediciny",
    type: "spec",
  },
  {
    title: "Врач УВТ",
    id: "7762",
    url: "vrachi/vrach-uvt",
    type: "spec",
  },
  {
    title: "Врач УЗИ",
    id: "8386",
    url: "vrachi/vrach-uzi",
    type: "spec",
  },
  {
    title: "Гастроэнтеролог",
    id: "7768",
    url: "vrachi/gastroenterolog",
    type: "spec",
  },
  {
    title: "Гериатр",
    id: "8374",
    url: "vrachi/geriatr",
    type: "spec",
  },
  {
    title: "Геронтолог",
    id: "8375",
    url: "vrachi/gerontolog",
    type: "spec",
  },
  {
    title: "Гинеколог",
    id: "8387",
    url: "vrachi/ginekolog",
    type: "spec",
  },
  {
    title: "Гинеколог-эндокринолог",
    id: "8388",
    url: "vrachi/ginekolog-endokrinolog",
    type: "spec",
  },
  {
    title: "Гипнотерапевт",
    id: "7771",
    url: "vrachi/gipnoterapevt",
    type: "spec",
  },
  {
    title: "Гирудолог",
    id: "7772",
    url: "vrachi/girudolog",
    type: "spec",
  },
  {
    title: "Гирудотерапевт",
    id: "7773",
    url: "vrachi/girudoterapevt",
    type: "spec",
  },
  {
    title: "Гомеопат",
    id: "7774",
    url: "vrachi/gomeopat",
    type: "spec",
  },
]);
</script>

<style scoped>
* {
  text-align: left;
}

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

.focused .dropdown-list {
  display: block;
}

.dropdown-list {
  position: absolute;
  display: none;
  z-index: 1;
  top: 8px;
  border-radius: 5px;
  box-shadow: 0px 14px 24px rgba(31, 49, 73, 0.25);
  width: 100%;
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
