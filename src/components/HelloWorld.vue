<script setup lang="ts">
import { onMounted, onUnmounted, ref, watch } from 'vue'
import { NSelect, NSpace } from 'naive-ui'

defineProps<{
  msg: string
}>()

const themeColorOptions = [
  {
    label: "OSの設定と連動する",
    value: 'os',
  },
  {
    label: 'ライトモード',
    value: 'light'
  },
  {
    label: 'ダークモード',
    value: 'dark'
  },
]

const editorColorOptions = [
  {
    label: "テーマカラーと連動する",
    value: 'theme',
  },
  {
    label: 'ストーン',
    value: 'stone'
  },
  {
    label: 'ブルーインク',
    value: 'blue ink'
  },
]
const mediaQuery = window.matchMedia('(prefers-color-scheme: dark)');


const themeColor = ref('os');
const isDark = ref(mediaQuery.matches);
const editorColor = ref('theme');

const updateDarkModePreference = (e: MediaQueryListEvent) => {
  isDark.value = e.matches;
};

const setTheme = () => {
  if (themeColor.value === 'dark') {
    document.documentElement.setAttribute('data-theme', 'dark');
  } else if (themeColor.value === 'light') {
    document.documentElement.removeAttribute('data-theme');
  } else { // OSの設定と連動の場合
    if (isDark.value) {
      document.documentElement.setAttribute('data-theme', 'dark');
    } else {
      document.documentElement.removeAttribute('data-theme');
    }
  }
};

onMounted(() => {
  mediaQuery.addEventListener('change', updateDarkModePreference);
});

onUnmounted(() => {
  mediaQuery.removeEventListener('change', updateDarkModePreference);
});

watch([themeColor, isDark], setTheme);

</script>

<template>
  <div class="greetings">
    <n-space vertical>
      <h1 class="green">{{ msg }}</h1>
      <h3>npm create vueで生成された雛形は既にダークモード対応であるが、HelloWord.vueで切り替え可能とした。</h3>
      <h4>OSの設定変更を検出しますが、設定値の保存はなし。</h4>
      <p>テーマカラー</p>
      <n-select v-model:value="themeColor" :options="themeColorOptions" />
      <p>エディタカラー</p>
      <n-select v-model:value="editorColor" :options="editorColorOptions" />
    </n-space>
  </div>
</template>

<style scoped>
h1 {
  font-weight: 500;
  font-size: 2.6rem;
  position: relative;
  top: -10px;
}

h3 {
  font-size: 1.2rem;
}

.greetings h1,
.greetings h3 {
  text-align: center;
}

@media (min-width: 1024px) {

  .greetings h1,
  .greetings h3 {
    text-align: left;
  }
}
</style>
