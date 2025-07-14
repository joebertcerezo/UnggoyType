<template>
  <div
    class="min-h-screen bg-gray-900 text-white flex flex-col items-center justify-center p-8 relative"
  >
    <div class="max-w-4xl w-full">
      <h1 class="text-3xl font-bold text-center mb-8 text-yellow-400">
        UnggoyType
      </h1>

      <div class="py-2 mb-6">
        <ul class="flex space-x-4 justify-center">
          <li
            v-for="count in wordCounts"
            :key="count"
            @click="selectWordCount(count)"
            :class="[
              'cursor-pointer px-4 py-2 rounded-lg font-medium transition-colors',
              selectedWordCount === count
                ? 'bg-yellow-500 text-black'
                : 'bg-gray-700 text-gray-300 hover:bg-gray-600',
            ]"
          >
            {{ count }}
          </li>
        </ul>
      </div>

      <!-- Typing Test Area -->
      <div class="bg-gray-800 rounded-lg p-8 mb-6">
        <div class="text-2xl leading-relaxed font-mono mb-6 select-none">
          <client-only>
            <span
              v-for="(char, index) in sampleText"
              :key="index"
              :class="getCharacterClass(index)"
              class="relative"
            >
              {{ char == " " ? "&nbsp;" : char }}
              <span
                v-if="index === currentIndex"
                class="absolute top-0 left-0 w-1 h-full bg-yellow-400 opacity-30 animate-pulse"
              ></span>
            </span>
          </client-only>
        </div>

        <!-- Stats -->
        <div class="flex justify-between items-center text-sm text-gray-400">
          <div>
            <span class="text-green-400">{{ correctChars }}</span> /
            <span class="text-red-400">{{ incorrectChars }}</span> /
            <span class="text-gray-400">{{ totalChars }}</span>
          </div>
          <div>
            Accuracy: <span class="text-blue-400">{{ accuracy }}%</span>
          </div>
          <div>
            WPM: <span class="text-purple-400">{{ wpm }}</span>
          </div>
        </div>
      </div>

      <!-- Reset and Generate Buttons -->
      <div class="text-center space-x-4">
        <button
          @click="resetTest"
          class="bg-yellow-500 hover:bg-yellow-600 text-black font-bold py-2 px-6 rounded transition-colors"
        >
          Reset Test
        </button>
        <button
          @click="generateNewWords"
          class="bg-blue-500 hover:bg-blue-600 text-white font-bold py-2 px-6 rounded transition-colors"
        >
          Generate New Words
        </button>
      </div>

      <!-- Virtual Keyboard (Optional) -->
      <!-- <div class="mt-8 grid grid-cols-10 gap-2 max-w-2xl mx-auto">
        <Key
          v-for="key in keyboardKeys"
          :key="key"
          :value="pressedKeys[key] ?? false"
        >
          {{ key }}
        </Key>
      </div> -->
    </div>
    <footer class="absolute bottom-0 py-5">
      <p class="text-sm text-gray-500 mt-4">
        &copy; {{ new Date().getFullYear() }} Joebert Cerezo. All rights
        reserved.
      </p>
    </footer>
  </div>
</template>

<script lang="ts" setup>
import { useMagicKeys } from "@vueuse/core";
import Key from "./components/Key.vue";
import randomWorkds from "@/constants/random-words.json";

const wordCounts = [10, 25, 50];
const selectedWordCount = ref(10);

// Make sampleText a reactive reference instead of computed
const sampleText = ref("");

// Function to generate new sample text
function generateSampleText() {
  const words = randomWorkds.words;
  const shuffled = [...words].sort(() => 0.5 - Math.random());
  return shuffled.slice(0, selectedWordCount.value).join(" ");
}

// Initialize sample text
sampleText.value = generateSampleText();

// Reactive state
const currentIndex = ref(0);
const userInput = ref("");
const startTime = ref<number | null>(null);
const endTime = ref<number | null>(null);
const errors = ref<boolean[]>([]);

// Initialize errors array
errors.value = new Array(sampleText.value.length).fill(false);

// Watch for changes in sampleText to reinitialize errors array
watch(
  sampleText,
  (newText) => {
    errors.value = new Array(newText.length).fill(false);
  },
  { immediate: true }
);

// Magic keys setup
const keys = useMagicKeys();
const pressedKeys = ref<Record<string, boolean>>({});

// Keyboard layout for display
const keyboardKeys = [
  "q",
  "w",
  "e",
  "r",
  "t",
  "y",
  "u",
  "i",
  "o",
  "p",
  "a",
  "s",
  "d",
  "f",
  "g",
  "h",
  "j",
  "k",
  "l",
  ";",
  "z",
  "x",
  "c",
  "v",
  "b",
  "n",
  "m",
  ",",
  ".",
  "/",
];

// Initialize all keys to false
keyboardKeys.forEach((key) => {
  pressedKeys.value[key] = false;
});
pressedKeys.value[" "] = false;

// Watch for key presses
keyboardKeys.forEach((key) => {
  watch(keys[key], (pressed) => {
    pressedKeys.value[key] = pressed;
    if (pressed) {
      handleKeyPress(key);
    }
  });
});

// Watch for space bar
watch(keys.space, (pressed) => {
  pressedKeys.value[" "] = pressed;
  if (pressed) {
    handleKeyPress(" ");
  }
});

// Watch for backspace
watch(keys.backspace, (pressed) => {
  if (pressed) {
    handleBackspace();
  }
});

// Handle key press
function handleKeyPress(key: string) {
  if (currentIndex.value >= sampleText.value.length) return;

  if (startTime.value === null) {
    startTime.value = Date.now();
  }

  const expectedChar = sampleText.value[currentIndex.value];
  const isCorrect = key === expectedChar;

  errors.value[currentIndex.value] = !isCorrect;
  userInput.value += key;

  // Always advance the cursor, whether correct or incorrect
  currentIndex.value++;

  // Check if test is complete
  if (currentIndex.value >= sampleText.value.length) {
    endTime.value = Date.now();
  }
}

// Handle backspace
function handleBackspace() {
  if (currentIndex.value > 0) {
    currentIndex.value--;
    userInput.value = userInput.value.slice(0, -1);
    errors.value[currentIndex.value] = false;
  }
}

// Get character styling class
function getCharacterClass(index: number) {
  if (index < currentIndex.value) {
    return errors.value[index]
      ? "text-red-400 bg-red-900 bg-opacity-30"
      : "text-green-400";
  } else if (index === currentIndex.value) {
    return "text-yellow-400 ";
  } else {
    return "text-gray-500";
  }
}

// Computed properties for stats
const correctChars = computed(() => {
  return errors.value.slice(0, currentIndex.value).filter((error) => !error)
    .length;
});

const incorrectChars = computed(() => {
  return errors.value.slice(0, currentIndex.value).filter((error) => error)
    .length;
});

const totalChars = computed(() => currentIndex.value);

const accuracy = computed(() => {
  if (totalChars.value === 0) return 100;
  return Math.round((correctChars.value / totalChars.value) * 100);
});

const wpm = computed(() => {
  if (!startTime.value || totalChars.value === 0) return 0;
  const endTimeValue = endTime.value || Date.now();
  const timeInMinutes = (endTimeValue - startTime.value) / 60000;
  const wordsTyped = correctChars.value / 5;
  return Math.round(wordsTyped / timeInMinutes);
});

// Reset test
function resetTest() {
  currentIndex.value = 0;
  userInput.value = "";
  startTime.value = null;
  endTime.value = null;
  errors.value = new Array(sampleText.value.length).fill(false);
}

// Generate new random words
function generateNewWords() {
  sampleText.value = generateSampleText();
  resetTest();
}

// Select word count for the test
function selectWordCount(count: number) {
  selectedWordCount.value = count;
  sampleText.value = generateSampleText();
  resetTest();
}
</script>
