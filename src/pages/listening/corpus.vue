<script setup>
import chapterData from './spelling_convention'

const playingWord = ref('')

function getWordName(word) {
  return Array.isArray(word) ? word[0] : word
}

function play(word) {
  const name = getWordName(word)
  playingWord.value = name
  const audio = document.createElement('audio')
  audio.src = `/179_audios/${name}.mp3`
  audio.onended = () => { playingWord.value = '' }
  audio.onerror = () => { playingWord.value = '' }
  audio.play().catch(() => { playingWord.value = '' })
}

const keyword = ref('')
const chapter = ref('Chapter2 拼写规范')
const sectionIndex = ref(0)
const chapters = [
  'Chapter2 拼写规范',
  'Chapter3 特别名词',
  'Chapter4 形容词副词',
  'Chapter5 吞音连读',
  'Chapter8 生存语料库',
  'Chapter11 综合测试',
  'Chapter12 8分核心词汇',
  'Chapter13 同义替换',
  'Chapter14 考点与趋势',
]

const curChapter = computed(() => chapterData[chapter.value])

const sections = computed(() => curChapter.value?.sections || [])

const curSection = computed(() => {
  const sec = sections.value[sectionIndex.value]
  if (sec) {
    sec.rows.forEach((e) => {
      if (typeof e[0] === 'string')
        e[0] = e[0].split(', ')
    })
  }
  return sec
})

function selectChapter(ch) {
  chapter.value = ch
  sectionIndex.value = 0
}
</script>

<template>
  <!-- Card header -->
  <div class="mt-6 items-center justify-between lg:flex">
    <div class="mb-4 lg:mb-0">
      <h3 class="mb-2 text-xl font-bold text-gray-900 dark:text-white">
        听力真题语料库
      </h3>
      <span class="text-base font-normal text-gray-500 dark:text-gray-400">包括各种词性、特殊训练</span>
    </div>
    <div class="items-center sm:flex">
      <div class="flex items-center">
        <select
          v-model="chapter"
          class="block w-full flex-1 border border-gray-300 rounded-lg bg-gray-50 p-2.5 text-sm text-gray-900 dark:border-gray-600 focus:border-blue-500 dark:bg-gray-700 dark:text-white focus:ring-blue-500 dark:focus:border-blue-500 dark:focus:ring-blue-500 dark:placeholder-gray-400"
          @change="selectChapter(chapter)"
        >
          <option
            v-for="k in chapters"
            :key="k"
            :value="k"
          >
            {{ k }}
          </option>
        </select>
        <div class="relative ml-2 flex-1">
          <div class="pointer-events-none absolute inset-y-0 left-0 flex items-center pl-3">
            <svg class="h-4 w-4 text-gray-500 dark:text-gray-400" aria-hidden="true" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 20 20">
              <path stroke="currentColor" stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="m19 19-4-4m0-7A7 7 0 1 1 1 8a7 7 0 0 1 14 0Z" />
            </svg>
          </div>
          <input
            v-model="keyword"
            type="search"
            class="block w-full border border-gray-300 rounded-lg bg-gray-50 p-2.5 pl-10 text-sm text-gray-900 dark:border-gray-600 focus:border-blue-500 dark:bg-gray-700 dark:text-white focus:ring-blue-500 dark:focus:border-blue-500 dark:focus:ring-blue-500 dark:placeholder-gray-400" placeholder="Search"
          >
        </div>
      </div>
    </div>
  </div>

  <!-- Section tabs -->
  <div v-if="sections.length > 1" class="mt-4 flex flex-wrap gap-2">
    <button
      v-for="(sec, idx) in sections"
      :key="idx"
      class="px-3 py-1.5 text-sm rounded-lg border transition-colors"
      :class="sectionIndex === idx
        ? 'bg-blue-600 text-white border-blue-600'
        : 'bg-white text-gray-600 border-gray-300 hover:bg-gray-100 dark:bg-gray-800 dark:text-gray-400 dark:border-gray-600 dark:hover:bg-gray-700'"
      @click="sectionIndex = idx"
    >
      {{ sec.title }}
    </button>
  </div>

  <div class="mt-6">
    <template v-if="curSection">
      <div class="mb-4 mt-6 items-center justify-between lg:flex">
        <div class="mb-4 lg:mb-0">
          <h3 class="mb-2 font-bold text-gray-900 dark:text-white">
            {{ curSection.title }}
          </h3>
          <span class="text-base font-normal text-gray-500 dark:text-gray-400">{{ curSection.desc }}</span>
        </div>
      </div>
      <table class="w-full text-left text-sm text-gray-500 dark:text-gray-400">
        <thead class="bg-gray-50 text-xs uppercase text-gray-700 dark:bg-gray-700 dark:text-gray-400">
          <tr>
            <th class="w-0 px-6 py-3">
              索引
            </th>
            <th class="w-0">
              <!-- Pronunciation -->
            </th>
            <th
              v-for="label in curSection.columns"
              :key="label"
              class="w-0 px-6 py-3"
            >
              {{ label }}
            </th>
          </tr>
        </thead>
        <tbody>
          <tr
            v-for="(row, index) in curSection.rows"
            :key="index"
            class="border-b bg-white dark:border-gray-700 dark:bg-gray-800"
          >
            <td class="px-6 py-4">
              {{ index + 1 }}
            </td>
            <td class="px-6 py-4">
              <a href="javascript:;" class="i-carbon-volume-up-filled block cursor-pointer hover:text-blue-500 transition-colors" :class="{ 'text-blue-500 animate-pulse': playingWord === getWordName(row[0]) }" @click="play(row[0])" />
            </td>
            <th class="whitespace-nowrap px-6 py-4 font-medium text-gray-600 dark:text-gray-300">
              <a
                class="hover:underline"
                :title="`在剑桥词典中查询 ${getWordName(row[0])}`"
                :href="`https://dictionary.cambridge.org/dictionary/english-chinese-simplified/${getWordName(row[0])}`"
                target="_blank"
              >{{ Array.isArray(row[0]) ? row[0].join(', ') : row[0] }}</a>
            </th>
            <td
              v-for="ci in curSection.columns.length - 1"
              :key="ci"
              class="whitespace-nowrap px-6 py-4"
              :class="{ 'text-gray-600 dark:text-gray-300': !curSection.columns[ci].includes('中文') &amp;&amp; !curSection.columns[ci].includes('词性'), 'text-gray-400 dark:text-gray-500': curSection.columns[ci].includes('中文') || curSection.columns[ci].includes('词性') }"
            >
              {{ row[ci] }}
            </td>
          </tr>
        </tbody>
      </table>
    </template>
    <div v-else class="text-center py-12 text-gray-500 dark:text-gray-400">
      暂无数据，敬请期待
    </div>
  </div>
</template>
