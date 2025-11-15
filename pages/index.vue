<template>
  <div class="min-h-screen bg-gray-900 relative overflow-x-hidden">
    <ClientOnly>
      <ParticleBackground />
    </ClientOnly>
    <!-- Main content section -->
    <div class="flex flex-col items-center justify-start pt-24 md:pt-32">
      <div class="text-6xl md:text-8xl text-gray-200 w-screen text-center relative z-10">Felix.</div>
      <div class="text-xl md:text-2xl text-gray-600 w-screen text-center relative z-10">Software Developer</div>
      <div class="w-screen flex justify-center mt-3 relative z-10">
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

    <!-- Projects Section -->
    <div class="w-full mt-20 px-4 md:px-8 pb-16 relative z-10">
      <div class="relative">
        <!-- Left scroll button -->
        <button @click="scrollProjects('left')" class="hidden md:block absolute left-0 top-1/2 -translate-y-1/2 z-20 bg-gray-800/90 hover:bg-gray-700/90 backdrop-blur-sm text-gray-200 rounded-full p-3 shadow-lg transition-all">
          <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6" fill="none" viewBox="0 0 24 24" stroke="currentColor">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M15 19l-7-7 7-7" />
          </svg>
        </button>
        
        <!-- Right scroll button -->
        <button @click="scrollProjects('right')" class="hidden md:block absolute right-0 top-1/2 -translate-y-1/2 z-20 bg-gray-800/90 hover:bg-gray-700/90 backdrop-blur-sm text-gray-200 rounded-full p-3 shadow-lg transition-all">
          <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6" fill="none" viewBox="0 0 24 24" stroke="currentColor">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 5l7 7-7 7" />
          </svg>
        </button>
        
      <div ref="projectsContainer" class="overflow-x-auto pb-4 scrollbar-hide scroll-smooth">
        <div class="flex space-x-6 px-4">
          <!-- Project Card 1 -->
          <div v-for="project in projects" :key="project.id" class="flex-none w-80 bg-gray-800/80 backdrop-blur-sm rounded-lg overflow-hidden flex flex-col">
            <!-- Image Section (1:1 aspect ratio) -->
            <div class="w-full h-40 bg-gray-700 overflow-hidden">
              <img :src="project.image" :alt="project.title" class="w-full h-full object-cover" />
            </div>
            
            <!-- Content Section -->
            <div class="p-5 flex flex-col flex-grow">
              <!-- Avatar List and Title Row -->
              <div class="mb-3">
                <h3 class="text-lg font-semibold text-gray-200 mb-2">{{ project.title }}</h3>
                <div class="flex items-center">
                  <div class="flex items-center mr-2">
                    <!-- Show first 3 avatars -->
                    <template v-for="(contributor, index) in project.contributors" :key="contributor.username">
                      <a v-if="index < 3" :href="`https://github.com/${contributor.username}`" target="_blank" class="relative inline-block -ml-2 first:ml-0">
                        <img :src="`https://github.com/${contributor.username}.png`" :alt="contributor.name" class="w-10 h-10 rounded-full ring-2 ring-gray-800 hover:ring-blue-500 hover:z-10 transition-all" :title="contributor.name" />
                      </a>
                    </template>
                    <!-- Show +X more button if there are more than 3 -->
                    <button v-if="project.contributors.length > 3" @click="expandedProject = expandedProject === project.id ? null : project.id" class="relative inline-block -ml-2 w-10 h-10 rounded-full ring-2 ring-gray-800 bg-gray-700 hover:bg-gray-600 hover:ring-blue-500 transition-all text-xs text-gray-300 font-semibold z-0 hover:z-10">
                      +{{ project.contributors.length - 3 }}
                    </button>
                  </div>
                </div>
                <!-- Expanded contributor list -->
                <div v-if="expandedProject === project.id" class="mt-3 p-3 bg-gray-700/80 backdrop-blur-sm rounded-lg">
                  <p class="text-xs text-gray-400 mb-2">All contributors:</p>
                  <div class="flex flex-wrap gap-2">
                    <a v-for="contributor in project.contributors" :key="contributor.username" :href="`https://github.com/${contributor.username}`" target="_blank" class="flex items-center bg-gray-600/80 backdrop-blur-sm hover:bg-gray-500/80 rounded-full pr-3 transition-colors">
                      <img :src="`https://github.com/${contributor.username}.png`" :alt="contributor.name" class="w-8 h-8 rounded-full ring-2 ring-gray-700" />
                      <span class="ml-2 text-sm text-gray-200">{{ contributor.name }}</span>
                    </a>
                  </div>
                </div>
              </div>
              
              <!-- Description -->
              <p class="text-sm text-gray-400 mb-4 flex-grow">{{ project.description }}</p>
              
              <!-- Links - Horizontal Scrollable - Always at bottom -->
              <div class="overflow-x-auto scrollbar-hide mt-auto">
                <div class="flex space-x-3">
                  <a v-for="link in project.links" :key="link.url" :href="link.url" target="_blank" class="flex-none text-blue-400 hover:text-blue-300 text-sm transition-colors whitespace-nowrap flex items-center gap-1">
                    {{ link.label }}
                    <svg xmlns="http://www.w3.org/2000/svg" class="h-4 w-4" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                      <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M10 6H6a2 2 0 00-2 2v10a2 2 0 002 2h10a2 2 0 002-2v-4M14 4h6m0 0v6m0-6L10 14" />
                    </svg>
                  </a>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
      </div>
    </div>
  </div>
</template>

<script setup>
const cvHover = ref(false)
const expandedProject = ref(null)
const projectsContainer = ref(null)
let timer = null

const setHoverTimer = () => {
  timer = setTimeout(() => {
    cvHover.value = false
  }, 300)
}

const clearHoverTimer = () => {
  clearTimeout(timer)
}

const scrollProjects = (direction) => {
  if (!projectsContainer.value) return
  const scrollAmount = 350 // Width of card + gap
  const currentScroll = projectsContainer.value.scrollLeft
  projectsContainer.value.scrollTo({
    left: direction === 'left' ? currentScroll - scrollAmount : currentScroll + scrollAmount,
    behavior: 'smooth'
  })
}

// Example projects data
const projects = ref([
  {
    title: 'RehPublic: Wildlife Monitoring',
    contributors: [
      { name: 'Felix', username: 'stromflix' },
      { name: 'Kristof', username: 'ArzelaAscoIi' },
      { name: 'Simon', username: 'nglhrdt' },
      { name: 'Markus', username: 'toybaer' },
      { name: 'Aaron', username: 'aaroneggert' },
    ],
    description: 'An AI-powered camera trap platform that automatically detects and maps wildlife, eliminating manual image review for biodiversity monitoring.',
    image: '/project_images/rehpublic.png',
    links: [
      { label: 'RehPublic Application', url: 'https://rehpublic.stromflix.com' },
    ]
  },
  {
    title: 'Build yout ETF',
    contributors: [
      { name: 'Felix', username: 'stromflix' }
    ],
    description: 'Build Your ETF is a web application that automatically optimizes ETF portfolio combinations to match your desired country and industry allocation preferences, helping investors construct diversified portfolios without manual research.',
    image: '/project_images/build-your-etf.png',
    links: [
      { label: 'Build Your ETF Application', url: 'https://build-your-etf.stromflix.com/' },
    ]
  },
  {
    title: 'Hallucination News',
    contributors: [
      { name: 'Felix', username: 'stromflix' }
    ],
    description: 'Automated system that monitors tabloid RSS feeds, uses AI to identify the most questionable and debunkable articles, performs fact-checking with real-time web research, and publishes verified analysis as Astro-based news content.',
    image: '/project_images/hallucination-news.png',
    links: [
      { label: 'Hallucination News', url: 'https://hallucination.news/' },
    ]
  },
  {
    title: 'Politrace',
    contributors: [
      { name: 'Felix', username: 'stromflix' }
    ],
    description: 'An AI tool that reads political party programs and reveals how different parties approach the same issues, making it easy to compare their proposed solutions and political stances side-by-side.',
    image: '/project_images/politrace.png',
    links: [
      { label: 'Politrace', url: 'https://politrace.stromflix.com/' },
    ]
  },
  {
    title: 'Indexadillo',
    contributors: [
      { name: 'Felix', username: 'stromflix' }
    ],
    description: 'Indexadillo is a scalable, observable Azure-based document indexing pipeline that uses Durable Functions to automatically extract, chunk, embed, and upload documents to Azure AI Search without restarting on failures.',
    image: '/project_images/indexadillo.png',
    links: [
      { label: 'Github: Indexadillo', url: 'https://github.com/Azure-Samples/indexadillo' },
    ]
  },
  {
    title: 'Factuality',
    contributors: [
      { name: 'Felix', username: 'stromflix' }
    ],
    description: 'Based on "long-form-factuality" a python based processor to easily fact check anything.',
    image: '/project_images/factuality.png',
    links: [
      { label: 'Github: Factuality', url: 'https://github.com/StromFLIX/factuality' },
    ]
  },
  {
    title: 'Obsidian',
    contributors: [
      { name: 'Felix', username: 'stromflix' }
    ],
    description: 'A fast, embeddable immutability layer that makes data auditable and tamper-proof. Obsidian enables any system to become a trusted participant in data ecosystems by ensuring information remains reliable and verifiable.',
    image: '/project_images/obsidian.png',
    links: [
      { label: 'Github: Obsidian', url: 'https://github.com/StromFLIX/obsidian' },
    ]
  },
])
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

/* Hide scrollbar but keep functionality */
.scrollbar-hide {
  -ms-overflow-style: none;  /* IE and Edge */
  scrollbar-width: none;  /* Firefox */
}

.scrollbar-hide::-webkit-scrollbar {
  display: none;  /* Chrome, Safari and Opera */
}

/* Line clamp for description text */
.line-clamp-3 {
  display: -webkit-box;
  -webkit-line-clamp: 3;
  -webkit-box-orient: vertical;
  overflow: hidden;
}
</style>
