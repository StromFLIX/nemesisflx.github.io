<template>
  <div class="h-screen flex flex-wrap content-center bg-gray-900">
    <div class="text-8xl text-gray-200 w-screen text-center">Felix.</div>
    <div class="text-2xl text-gray-600 w-screen text-center">Software Developer</div>
    <div class="w-screen flex justify-center mt-3">
      <button class="m-2 p-1 px-2 shadow-lg rounded-full bg-blue-500">
        <a href="mailto:felix.moenckemeyer@gmail.com">
          <font-awesome-icon style="font-size: 16px" :icon="['fas', 'envelope']" />
        </a>
      </button>
      <button class="m-2 rounded-full shadow-lg bg-blue-500" style="padding: 0px 9px 0px 9px">
        <a href="https://www.linkedin.com/in/felix-m%C3%B6nckemeyer-6887a61a0/">
          <font-awesome-icon style="font-size: 16px" :icon="['fab', 'linkedin-in']" />
        </a>
      </button>
      <div class="relative">
        <div class="m-1" @mouseenter="cvHover = true" @mouseleave="setHoverTimer()">
          <div class="p-1 bg-blue-500 shadow-lg rounded-full">
            <div class="rounded-full bg-gray-200" style="padding: 4px 10px 4px 10px">
              <font-awesome-icon style="font-size: 16px" :icon="['fas', 'file-alt']" />
            </div>
          </div>
        </div>
        <div v-if="cvHover" class="absolute w-96" style="left: 50%; transform: translateX(-50%)" @mouseenter="clearHoverTimer()" @mouseleave="cvHover = false">
          <div class="arrow absolute mt-0" style="left: 50%; transform: translateX(-50%)" />
          <div class="bg-gray-100 rounded p-0 m-2 ">
            <div>
              <div class="relative grid gap-5 rounded bg-white p-5 ">
                <button>
                  <a href="https://github.com/NemesisFLX/CV/raw/main/german/cv_felix_moenckemeyer_de.pdf" class="flex flex-row items-center hover:bg-gray-100 p-2 rounded">
                    <div class="w-8">
                      <svg xmlns="http://www.w3.org/2000/svg" id="flag-icon-css-de" viewBox="0 0 640 480">
                        <path fill="#ffce00" d="M0 320h640v160H0z"/>
                        <path d="M0 0h640v160H0z"/>
                        <path fill="#d00" d="M0 160h640v160H0z"/>
                      </svg>
                    </div>
                    <div class="flex flex-col ml-3">
                      <span class="text-sm text-left font-semibold">German</span>
                      <span class="text-sm text-gray-600">Lebenslauf herunterladen</span>
                    </div>
                    <div class="ml-auto mr-2 text-gray-600">
                      <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 10v6m0 0l-3-3m3 3l3-3m2 8H7a2 2 0 01-2-2V5a2 2 0 012-2h5.586a1 1 0 01.707.293l5.414 5.414a1 1 0 01.293.707V19a2 2 0 01-2 2z" />
                      </svg>
                    </div>
                  </a>
                </button>
                <button>
                  <a href="https://github.com/NemesisFLX/CV/raw/main/english/cv_felix_moenckemeyer_en.pdf" class="flex flex-row items-center hover:bg-gray-100 p-2 rounded">
                    <div class="w-8">
                      <svg xmlns="http://www.w3.org/2000/svg" id="flag-icon-css-gb" viewBox="0 0 640 480">
                        <path fill="#012169" d="M0 0h640v480H0z"/>
                        <path fill="#FFF" d="M75 0l244 181L562 0h78v62L400 241l240 178v61h-80L320 301 81 480H0v-60l239-178L0 64V0h75z"/>
                        <path fill="#C8102E" d="M424 281l216 159v40L369 281h55zm-184 20l6 35L54 480H0l240-179zM640 0v3L391 191l2-44L590 0h50zM0 0l239 176h-60L0 42V0z"/>
                        <path fill="#FFF" d="M241 0v480h160V0H241zM0 160v160h640V160H0z"/>
                        <path fill="#C8102E" d="M0 193v96h640v-96H0zM273 0v480h96V0h-96z"/>
                      </svg>
                    </div>
                    <div class="flex flex-col ml-3">
                      <span class="text-sm text-left font-semibold">English</span>
                      <span class="text-sm text-gray-600">Download resume</span>
                    </div>
                    <div class="ml-auto mr-2 text-gray-600">
                      <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 10v6m0 0l-3-3m3 3l3-3m2 8H7a2 2 0 01-2-2V5a2 2 0 012-2h5.586a1 1 0 01.707.293l5.414 5.414a1 1 0 01.293.707V19a2 2 0 01-2 2z" />
                      </svg>
                    </div>
                  </a>
                </button>
              </div>
              <div class="flex flex-col m-0 p-3 bg-gray-100 rounded">
                <span class="text-lg font">Resume</span>
                <span class="text-sm text-gray-600">Find all information of my educational and work experience</span>
              </div>
            </div>
          </div>
        </div>
      </div>
      <button class="m-2 p-1 px-2 shadow-lg rounded-full bg-blue-500">
        <a href="https://github.com/NemesisFLX">
          <font-awesome-icon style="font-size: 16px" :icon="['fab', 'github']" />
        </a>
      </button>
      <button class="m-2 rounded-full shadow-lg bg-blue-500" style="padding: 0px 6px 0px 6px">
        <a href="https://discordapp.com/users/582992527288696874/">
          <font-awesome-icon style="font-size: 16px" :icon="['fab', 'discord']" />
        </a>
      </button>
    </div>
  </div>
</template>

<script setup>
const cvHover = ref(false)
let timer = null

const setHoverTimer = () => {
  timer = setTimeout(() => {
    cvHover.value = false
  }, 300)
}

const clearHoverTimer = () => {
  clearTimeout(timer)
}
</script>

<style>
.arrow {
  width: 0;
  height: 0;
  margin-top: 2px;
  border-left: 6px solid transparent;
  border-right: 6px solid transparent;
  border-bottom: 6px solid #F1F5F9;
}
</style>
