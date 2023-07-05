<template>
  <div class="w-screen h-screen flex justify-center items-center">
    <div class="flex flex-col justify-center items-center gap-3">
      <h2 class="text-2xl font-light">Round: {{ rounds }} Exercise: {{ exercise }}</h2>
      <h1 ref="clock" class="text-8xl font-medium">{{ seconds }}</h1>
      <div class="flex justify-center items-center gap-3">
        <button
          ref="control"
          class="w-32 h-10 rounded-lg text-xl font-medium bg-green-500"
          @click="handleControl"
        >
          Start
        </button>
        <button
          class="w-32 h-10 flex justify-center items-center gap-1 rounded-lg text-xl font-medium disabled:text-gray-400 bg-gray-300"
          @click="openSettings"
          :disabled="isStarted"
        >
          <svg
            xmlns="http://www.w3.org/2000/svg"
            fill="none"
            viewBox="0 0 24 24"
            stroke-width="1.5"
            stroke="currentColor"
            class="w-6 h-6"
          >
            <path
              stroke-linecap="round"
              stroke-linejoin="round"
              d="M10.343 3.94c.09-.542.56-.94 1.11-.94h1.093c.55 0 1.02.398 1.11.94l.149.894c.07.424.384.764.78.93.398.164.855.142 1.205-.108l.737-.527a1.125 1.125 0 011.45.12l.773.774c.39.389.44 1.002.12 1.45l-.527.737c-.25.35-.272.806-.107 1.204.165.397.505.71.93.78l.893.15c.543.09.94.56.94 1.109v1.094c0 .55-.397 1.02-.94 1.11l-.893.149c-.425.07-.765.383-.93.78-.165.398-.143.854.107 1.204l.527.738c.32.447.269 1.06-.12 1.45l-.774.773a1.125 1.125 0 01-1.449.12l-.738-.527c-.35-.25-.806-.272-1.203-.107-.397.165-.71.505-.781.929l-.149.894c-.09.542-.56.94-1.11.94h-1.094c-.55 0-1.019-.398-1.11-.94l-.148-.894c-.071-.424-.384-.764-.781-.93-.398-.164-.854-.142-1.204.108l-.738.527c-.447.32-1.06.269-1.45-.12l-.773-.774a1.125 1.125 0 01-.12-1.45l.527-.737c.25-.35.273-.806.108-1.204-.165-.397-.505-.71-.93-.78l-.894-.15c-.542-.09-.94-.56-.94-1.109v-1.094c0-.55.398-1.02.94-1.11l.894-.149c.424-.07.765-.383.93-.78.165-.398.143-.854-.107-1.204l-.527-.738a1.125 1.125 0 01.12-1.45l.773-.773a1.125 1.125 0 011.45-.12l.737.527c.35.25.807.272 1.204.107.397-.165.71-.505.78-.929l.15-.894z"
            />
            <path
              stroke-linecap="round"
              stroke-linejoin="round"
              d="M15 12a3 3 0 11-6 0 3 3 0 016 0z"
            />
          </svg>

          Settings
        </button>
      </div>

      <audio ref="music"><source type="audio/mpeg" src="./assets/beep.mp3"></audio>
      <audio ref="music2"><source type="audio/mpeg" src="./assets/beep2.mp3"></audio>
      <audio ref="music3"><source type="audio/mpeg" src="./assets/beep3.mp3"></audio>
    </div>
    <TransitionRoot appear :show="areSettingsOpen" as="template">
      <Dialog as="div" @close="closeSettings" class="relative z-10">
        <TransitionChild
          as="template"
          enter="duration-300 ease-out"
          enter-from="opacity-0"
          enter-to="opacity-100"
          leave="duration-200 ease-in"
          leave-from="opacity-100"
          leave-to="opacity-0"
        >
          <div class="fixed inset-0 bg-black bg-opacity-25" />
        </TransitionChild>

        <div class="fixed inset-0 overflow-y-auto">
          <div
            class="flex min-h-full items-center justify-center p-4 text-center"
          >
            <TransitionChild
              as="template"
              enter="duration-300 ease-out"
              enter-from="opacity-0 scale-95"
              enter-to="opacity-100 scale-100"
              leave="duration-200 ease-in"
              leave-from="opacity-100 scale-100"
              leave-to="opacity-0 scale-95"
            >
              <DialogPanel
                class="w-full max-w-md transform overflow-hidden rounded-2xl bg-white p-6 text-left align-middle shadow-xl transition-all"
              >
                <DialogTitle
                  as="h3"
                  class="text-lg font-medium leading-6 text-gray-900"
                >
                  Settings
                </DialogTitle>
                <div class="mt-2">
                  <div class="flex justify-start items-center gap-2 py-2">
                    <p class="w-16">Seconds:</p>
                    <input v-model="secondsForm" type="text" class="px-2 bg-gray-100 rounded-md border border-transparent text-gray-900 hover:bg-gray-200 active:bg-gray-200 focus:outline-none focus-visible:ring-2 focus-visible:ring-gray-500 focus-visible:ring-offset-2" />
                  </div>
                  <div class="flex justify-start items-center gap-2 py-2">
                    <p class="w-16">Rest:</p>
                    <input v-model="restForm" type="text" class="px-2 bg-gray-100 rounded-md border border-transparent text-gray-900 hover:bg-gray-200 active:bg-gray-200 focus:outline-none focus-visible:ring-2 focus-visible:ring-gray-500 focus-visible:ring-offset-2" />
                  </div>
                  <div class="flex justify-start items-center gap-2 py-2">
                    <p class="w-16">Rounds:</p>
                    <input v-model="roundsForm" type="text" class="px-2 bg-gray-100 rounded-md border border-transparent text-gray-900 hover:bg-gray-200 active:bg-gray-200 focus:outline-none focus-visible:ring-2 focus-visible:ring-gray-500 focus-visible:ring-offset-2" />
                  </div>
                  <div class="flex justify-start items-center gap-2 py-2">
                    <p class="w-16">Exercises:</p>
                    <input v-model="exerciseForm" type="text" class="px-2 bg-gray-100 rounded-md border border-transparent text-gray-900 hover:bg-gray-200 active:bg-gray-200 focus:outline-none focus-visible:ring-2 focus-visible:ring-gray-500 focus-visible:ring-offset-2" />
                  </div>
                </div>

                <div class="mt-4 flex justify-start items-center gap-3">
                  <button
                    type="button"
                    class="inline-flex justify-center rounded-md border border-transparent bg-gray-100 px-4 py-2 text-sm font-medium text-gray-900 hover:bg-gray-200 focus:outline-none focus-visible:ring-2 focus-visible:ring-gray-500 focus-visible:ring-offset-2"
                    @click="closeSettings"
                  >
                    Cancel
                  </button>
                  <button 
                    type="button" 
                    class="inline-flex justify-center rounded-md border border-transparent bg-blue-100 px-4 py-2 text-sm font-medium text-blue-900 hover:bg-blue-200 focus:outline-none focus-visible:ring-2 focus-visible:ring-blue-500 focus-visible:ring-offset-2"
                    @click="saveSettings"
                  >
                    Save
                  </button>
                </div>
              </DialogPanel>
            </TransitionChild>
          </div>
        </div>
      </Dialog>
    </TransitionRoot>
  </div>
</template>

<script setup>
import {
  TransitionRoot,
  TransitionChild,
  Dialog,
  DialogPanel,
  DialogTitle,
} from "@headlessui/vue";

const areSettingsOpen = ref(false);
const clock = ref(null);
const control = ref(null);
const exercise = ref(0);
const exerciseForm = ref("");
const isStarted = ref(false);
const music = ref(null);
const music2 = ref(null);
const music3 = ref(null);
const restForm = ref("");
const rounds = ref(1)
const roundsForm = ref("");
const seconds = ref(40);
const secondsForm = ref("");

let desiredExercises = 4;
let desiredRest = 20;
let desiredRounds = 5;
let desiredSeconds = 40;
let timer = null;

function closeSettings() {
  areSettingsOpen.value = false;
}

function handleControl() {
  if (isStarted.value) stop();
  else {
    isStarted.value = true;
    start();
  }
}

function openSettings() {
  areSettingsOpen.value = true;
}

function rest() {
  seconds.value = desiredRest;
  if (!clock.value.classList?.contains("text-green-500"))
    clock.value.classList?.add("text-green-500");

  timer = setInterval(() => {
    if (seconds.value === 0) {
      clock.value.classList?.remove("text-green-500");
      clearInterval(timer);
      timer = null;

      if (rounds.value < desiredRounds)
        start();
      else 
        stop();
    } else {
      seconds.value -= 1;
    }
  }, 1000);
}

function saveSettings() {
  desiredSeconds = parseInt(secondsForm.value);
  desiredRest = parseInt(restForm.value);
  desiredRounds = parseInt(roundsForm.value);
  desiredExercises = parseInt(exerciseForm.value);

  seconds.value = desiredSeconds;

  closeSettings();
}

function start() {
  control.value.classList?.remove("bg-green-500");
  control.value.classList?.add("bg-red-500");
  control.value.innerHTML = "Stop";

  if (exercise.value === desiredExercises) {
    rounds.value += 1;
    exercise.value = 1;
  } else
    exercise.value += 1;
  
  seconds.value = desiredSeconds;

  music3.value.play();

  timer = setInterval(() => {
    if (seconds.value === 0) {
      clock.value.classList?.remove("text-red-500");
      clearInterval(timer);
      timer = null;

      music2.value.play();
      rest();
    } else {
      if (seconds.value <= 11 && !clock.value.classList?.contains("text-red-500")) {
        clock.value.classList?.add("text-red-500");
        music.value.play();
      }

      seconds.value -= 1;
    }
  }, 1000);
}

function stop() {
  clearInterval(timer);
  timer = null;

  control.value.classList?.remove("bg-red-500");
  control.value.classList?.add("bg-green-500");
  control.value.innerHTML = "Start";

  if (clock.value.classList?.contains("text-red-500"))
    clock.value.classList?.remove("text-red-500");

  if (clock.value.classList?.contains("text-green-500"))
    clock.value.classList?.remove("text-green-500");

  isStarted.value = false;

  seconds.value = desiredSeconds;
  rounds.value = 1;
  exercise.value = 0;
}
</script>
