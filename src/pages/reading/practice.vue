<script setup>
import categories from './reading538words'

const allWords = categories.flatMap(cat =>
  cat.words.map(w => ({
    index: w[0],
    word: w[1],
    pos: w[2],
    meaning: w[3].join('；'),
    replace: w[4],
    category: cat.title,
  }))
)

const ws = reactive(allWords.map((v) => ({
  ...v,
  form: { word: '' },
  checked: false,
  correct: false,
})))

const playingWord = ref('')

let audio = null
function play(word) {
  if (audio) {
    audio.pause()
    audio.currentTime = 0
  }
  playingWord.value = word
  audio = document.createElement('audio')
  audio.src = `https://dict.youdao.com/dictvoice?audio=${word}&type=1`
  audio.onended = () => { playingWord.value = '' }
  audio.onerror = () => { playingWord.value = '' }
  audio.play().catch(() => { playingWord.value = '' })
}

function onKeydown(e, word) {
  if (e.key === '`') {
    e.preventDefault()
    play(word)
  }
}

function check(index) {
  const cw = ws[index]
  const input = cw.form.word.trim().toLowerCase()
  cw.checked = true

  if (input === cw.word) {
    cw.correct = true
    const i = index + 1
    if (i >= allWords.length) return
    const nw = allWords[i]
    play(nw.word)
    document.getElementById(`input_${nw.index}`)?.focus()
  } else {
    cw.correct = false
  }
}
</script>

<template>
  <div>
    <div class="mt-6 items-center justify-between lg:flex">
      <div class="mb-4 lg:mb-0">
        <h3 class="mb-2 text-xl font-bold text-gray-900 dark:text-white">
          阅读 538 考点词练习
        </h3>
        <ul class="ml-4 list-decimal text-sm font-normal text-gray-500 dark:text-gray-400">
          <li>音频会在切换下一个词、点击喇叭图标、按键 <kbd class="rounded-lg bg-gray-100 px-2 text-xs font-semibold text-gray-800 dark:border-gray-500 dark:bg-gray-600 dark:text-gray-100">`</kbd>（数字 1 左边那个）播放</li>
          <li>根据中文提示输入考点词，按 Enter 自动检查，正确跳转下一词，错误停留在当前词</li>
        </ul>
      </div>
      <div class="items-center sm:flex">
        <div class="flex items-center">
          <button
            type="button"
            class="rounded-lg bg-blue-700 px-5 py-2.5 text-sm font-medium text-white dark:bg-blue-600 hover:bg-blue-800 focus:outline-none focus:ring-4 focus:ring-blue-300 dark:hover:bg-blue-700 dark:focus:ring-blue-800"
            @click="() => { play(allWords[0].word) }"
          >
            开始
          </button>
        </div>
      </div>
    </div>
    <div class="relative mt-4 overflow-x-auto">
      <table class="w-full text-left text-sm text-gray-500 dark:text-gray-400">
        <thead class="bg-gray-50 text-xs uppercase text-gray-700 dark:bg-gray-700 dark:text-gray-400">
          <tr>
            <th class="w-0 px-6 py-3">索引</th>
            <th class="w-20 px-6 py-3">音频</th>
            <th class="w-0 px-6 py-3">词性</th>
            <th class="px-6 py-3">考点词</th>
            <th class="px-6 py-3">中文</th>
            <th class="px-6 py-3">同义替换</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(w, i) in ws" :key="w.index" class="border-b bg-white dark:border-gray-700 dark:bg-gray-800">
            <td class="px-6 py-4">{{ w.index }}</td>
            <td class="px-6 py-4">
              <button class="i-carbon-volume-up-filled cursor-pointer hover:text-blue-500 transition-colors" :class="{ 'text-blue-500 animate-pulse': playingWord === w.word }" @click="play(w.word)" />
            </td>
            <td class="px-6 py-4 italic text-gray-400 dark:text-gray-500">{{ w.pos.join('、') }}</td>
            <td class="px-6 py-4" @keydown="onKeydown($event, w.word)">
              <input
                :id="`input_${w.index}`"
                v-model="w.form.word"
                p="x-2 y-1"
                w="150px"
                bg="transparent"
                spellcheck="false"
                type="text"
                placeholder="请输入..."
                :style="w.checked ? (w.correct ? 'border:2px solid #86efac;border-radius:0.375rem' : 'border:2px solid #fca5a5;border-radius:0.375rem') : 'border:1px solid #d1d5db;border-radius:0.375rem'"
                class="text-gray-900 dark:text-white"
                @keydown.enter="check(i)"
              >
              <span v-if="w.checked && !w.correct" class="ml-2 text-xs text-red-500">{{ w.word }}</span>
            </td>
            <td class="px-6 py-4 text-gray-600 dark:text-gray-300">{{ w.meaning }}</td>
            <td class="px-6 py-4 text-gray-600 dark:text-gray-300">{{ w.replace.join(', ') }}</td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>
