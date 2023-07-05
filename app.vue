<template>
  <div class="w-screen h-screen flex justify-center items-center">
    <div class="flex flex-col justify-center items-center gap-3">
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
          class="w-32 h-10 flex justify-center items-center gap-1 rounded-lg text-xl font-medium bg-gray-300"
          @click="openSettings"
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
                  Payment successful
                </DialogTitle>
                <div class="mt-2">
                  <p class="text-sm text-gray-500">
                    Your payment has been successfully submitted. We’ve sent you
                    an email with all of the details of your order.
                  </p>
                </div>

                <div class="mt-4">
                  <button
                    type="button"
                    class="inline-flex justify-center rounded-md border border-transparent bg-blue-100 px-4 py-2 text-sm font-medium text-blue-900 hover:bg-blue-200 focus:outline-none focus-visible:ring-2 focus-visible:ring-blue-500 focus-visible:ring-offset-2"
                    @click="closeSettings"
                  >
                    Got it, thanks!
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
const seconds = ref(40);

let isStarted = false;
let timer = null;

function closeSettings() {
  areSettingsOpen.value = false;
}

function handleControl() {
  if (isStarted) stop();
  else start();

  isStarted = !isStarted;
}

function openSettings() {
  areSettingsOpen.value = true;
}

function rest() {
  seconds.value = 20;
  if (!clock.value.classList.contains("text-green-500"))
    clock.value.classList.add("text-green-500");

  timer = setInterval(() => {
    if (seconds.value === 0) {
      clock.value.classList.remove("text-green-500");
      clearInterval(timer);
      timer = null;
    } else {
      seconds.value -= 1;
    }
  }, 1000);
}

function start() {
  control.value.classList.remove("bg-green-500");
  control.value.classList.add("bg-red-500");
  control.value.innerHTML = "Stop";

  timer = setInterval(() => {
    if (seconds.value === 0) {
      clock.value.classList.remove("text-red-500");
      clearInterval(timer);
      timer = null;
      rest();
    } else {
      if (seconds.value < 11 && !clock.value.classList.contains("text-red-500"))
        clock.value.classList.add("text-red-500");
      seconds.value -= 1;
    }
  }, 1000);
}

function stop() {
  clearInterval(timer);
  timer = null;

  control.value.classList.remove("bg-red-500");
  control.value.classList.add("bg-green-500");
  control.value.innerHTML = "Start";

  if (clock.value.classList.contains("text-red-500"))
    clock.value.classList.remove("text-red-500");

  if (clock.value.classList.contains("text-green-500"))
    clock.value.classList.remove("text-green-500");

  seconds.value = 40;
}

function saveSettings() {}
</script>
