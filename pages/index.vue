<template>
  <div class="flex bg-zinc-900 h-screen relative">
    <!-- Sidebar principal desktop -->
    <div
      v-if="!showNotesSidebar"
      class="hidden md:flex bg-black w-[338px] p-8 flex flex-col justify-between"
    >
      <div>
        <Logo />
        <h1 class="text-white font-bold text-md mt-8">Menu</h1>

        <button
          class="mt-4 w-full bg-zinc-800 hover:bg-zinc-700 transition-colors text-white text-sm font-medium py-2 rounded-lg text-center cursor-pointer"
          @click="onDateClick"
        >
          {{ currentDate }}
        </button>

        <div class="mt-4 border-t border-zinc-700 w-full"></div>

        <button
          @click="openNotesSidebar"
          class="mt-4 w-full bg-zinc-800 hover:bg-zinc-700 transition-colors text-white text-sm font-medium py-2 rounded-lg"
        >
          Notas
        </button>
        <button
          class="mt-4 w-full bg-zinc-800 hover:bg-zinc-700 transition-colors text-white text-sm font-medium py-2 rounded-lg"
        >
          Arquivos
        </button>
      </div>

      <div class="relative mt-6">
        <button
          @click="toggleDropdown"
          class="w-12 h-12 bg-zinc-700 rounded-full flex items-center justify-center hover:bg-zinc-600 transition-colors"
        >
          <UserIcon class="h-5 w-5 text-white" />
        </button>

        <transition name="fade-up">
          <div
            v-if="showDropdown"
            class="absolute bottom-14 left-0 w-48 bg-zinc-800 rounded-lg shadow-xl py-2 z-50 animate-dropdown"
          >
            <a
              href="#"
              class="flex items-center gap-2 px-4 py-2 text-sm text-white hover:bg-zinc-700 transition-colors"
            >
              <UserIcon class="w-4 h-4" /> Perfil
            </a>
            <a
              href="#"
              class="flex items-center gap-2 px-4 py-2 text-sm text-white hover:bg-zinc-700 transition-colors"
            >
              <SettingsIcon class="w-4 h-4" /> Configurações
            </a>
            <div class="border-t border-zinc-700 my-2"></div>
            <button
              @click="logout"
              class="flex items-center gap-2 w-full text-left px-4 py-2 text-sm text-red-500 hover:bg-zinc-700 transition-colors"
            >
              <LogOutIcon class="w-4 h-4" /> Sair
            </button>
          </div>
        </transition>
      </div>
    </div>

    <!-- Sidebar principal drawer mobile -->
    <transition name="fade-up">
      <div
        v-if="showMainSidebar && isMobile"
        class="fixed inset-0 bg-black bg-opacity-90 z-50 p-8 flex flex-col justify-between md:hidden"
      >
        <div>
          <Logo />
          <h1 class="text-white font-bold text-md mt-8">Menu</h1>

          <button
            class="mt-4 w-full bg-zinc-800 hover:bg-zinc-700 transition-colors text-white text-sm font-medium py-2 rounded-lg text-center cursor-pointer"
            @click="onDateClick"
          >
            {{ currentDate }}
          </button>

          <div class="mt-4 border-t border-zinc-700 w-full"></div>

          <button
            @click="openNotesSidebar"
            class="mt-4 w-full bg-zinc-800 hover:bg-zinc-700 transition-colors text-white text-sm font-medium py-2 rounded-lg"
          >
            Notas
          </button>
          <button
            class="mt-4 w-full bg-zinc-800 hover:bg-zinc-700 transition-colors text-white text-sm font-medium py-2 rounded-lg"
          >
            Arquivos
          </button>
        </div>

        <div class="mt-auto flex justify-end">
          <button
            @click="closeMainSidebar"
            class="w-12 h-12 bg-zinc-700 rounded-full flex items-center justify-center hover:bg-zinc-600 transition-colors"
            title="Fechar menu"
          >
            <XIcon class="h-5 w-5 text-white" />
          </button>
        </div>
      </div>
    </transition>

    <!-- Sidebar notas desktop -->
    <div
      v-if="showNotesSidebar && !isMobile"
      class="hidden md:flex bg-black w-[338px] h-screen p-8 flex flex-col justify-between"
    >
      <div>
        <Logo />

        <div>
          <p class="text-xs font-bold text-[#C2C2C5] mt-12 mb-4">Hoje</p>
          <div class="ml-2">
            <div class="p-2 bg-[#6c18bf] rounded-lg">
              <h3 class="text-sm font-bold text-[#F4F4F5]">
                Projeto AcademicHub
              </h3>
              <div class="leading-snug">
                <span class="text-xs text-[#F4F4F5] mr-4">Hoje</span>
                <span class="text-xs text-[#C2C2C5]">AcademicHub</span>
              </div>
            </div>
            <div class="p-2">
              <h3 class="text-sm font-bold text-[#F4F4F5]">Terminei de ler</h3>
              <div class="leading-snug">
                <span class="text-xs text-[#F4F4F5] mr-4">Hoje</span>
                <span class="text-xs text-[#C2C2C5]">Codigo davinci</span>
              </div>
            </div>
          </div>
        </div>

        <div>
          <p class="text-xs font-bold text-[#C2C2C5] mt-12 mb-4">Ontem</p>
          <div class="ml-2">
            <div class="p-2 rounded-lg">
              <h3 class="text-sm font-bold text-[#F4F4F5]">Projeto Codify</h3>
              <div class="leading-snug">
                <span class="text-xs text-[#F4F4F5] mr-4">Ontem</span>
                <span class="text-xs text-[#C2C2C5]">Dei commit</span>
              </div>
            </div>
          </div>
        </div>
      </div>
      <div class="mt-auto">
        <button
          @click="closeNotesSidebar"
          class="w-12 h-12 bg-zinc-700 rounded-full flex items-center justify-center hover:bg-zinc-600 transition-colors"
          title="Fechar notas"
        >
          <XIcon class="h-5 w-5 text-white" />
        </button>
      </div>
    </div>

    <!-- Sidebar notas drawer mobile -->
    <transition name="fade-up">
      <div
        v-if="showNotesSidebar && isMobile"
        class="fixed inset-0 bg-black bg-opacity-90 z-50 p-8 flex flex-col justify-between md:hidden"
      >
        <div>
          <Logo />
          <div class="mt-12">
            <p class="text-xs font-bold text-[#C2C2C5] mb-4">Hoje</p>
            <div class="ml-2">
              <div class="p-2 bg-[#6c18bf] rounded-lg">
                <h3 class="text-sm font-bold text-[#F4F4F5]">
                  Projeto AcademicHub
                </h3>
                <div class="leading-snug">
                  <span class="text-xs text-[#F4F4F5] mr-4">Hoje</span>
                  <span class="text-xs text-[#C2C2C5]">AcademicHub</span>
                </div>
              </div>
              <div class="p-2">
                <h3 class="text-sm font-bold text-[#F4F4F5]">Terminei de ler</h3>
                <div class="leading-snug">
                  <span class="text-xs text-[#F4F4F5] mr-4">Hoje</span>
                  <span class="text-xs text-[#C2C2C5]">Codigo davinci</span>
                </div>
              </div>
            </div>

            <p class="text-xs font-bold text-[#C2C2C5] mt-12 mb-4">Ontem</p>
            <div class="ml-2">
              <div class="p-2 rounded-lg">
                <h3 class="text-sm font-bold text-[#F4F4F5]">Projeto Codify</h3>
                <div class="leading-snug">
                  <span class="text-xs text-[#F4F4F5] mr-4">Ontem</span>
                  <span class="text-xs text-[#C2C2C5]">Dei commit</span>
                </div>
              </div>
            </div>
          </div>
        </div>
        <div class="mt-auto flex justify-end">
          <button
            @click="closeNotesSidebar"
            class="w-12 h-12 bg-zinc-700 rounded-full flex items-center justify-center hover:bg-zinc-600 transition-colors"
            title="Fechar notas"
          >
            <XIcon class="h-5 w-5 text-white" />
          </button>
        </div>
      </div>
    </transition>

    <!-- Calendário e conteúdo principal -->
    <div
      :class="[
        'h-screen bg-zinc-900 overflow-hidden',
        isMobile && !showNotesSidebar && !showMainSidebar ? 'w-full' : 'flex-1',
        'rounded-l-lg',
      ]"
    >
      <!-- Botões abrir sidebars topo calendário (mobile) -->
      <div class="flex items-center justify-between p-4 md:hidden bg-zinc-800 text-white">
        <button
          @click="openMainSidebar"
          class="flex items-center space-x-2 text-sm font-medium"
        >
          <PencilIcon class="w-5 h-5" />
          <span>Menu</span>
        </button>
      </div>

      <!-- Conteúdo notas desktop -->
      <div v-if="showNotesSidebar && !isMobile" class="h-full p-8 overflow-auto">
        <p class="text-[#929292] font-playfair">06 de Abril de 2025</p>
        <p class="text-[#D4D4D4] my-4 font-playfair">
          O AcademicHub é uma aplicação web desenvolvida para auxiliar estudantes na organização da rotina escolar. Ele combina funcionalidades de calendário e anotações, permitindo que os usuários visualizem datas importantes — como provas e trabalhos — e criem suas próprias notas personalizadas. O diferencial do AcademicHub é a possibilidade de edição colaborativa do calendário por todos os usuários, promovendo uma gestão mais participativa do ambiente escolar.
        </p>
        <p class="text-[#D4D4D4] my-4 font-playfair">
          Lorem ipsum dolor sit amet consectetur adipisicing elit...
        </p>
      </div>

      <!-- Conteúdo calendário principal -->
      <div
        v-if="!showNotesSidebar && !showMainSidebar"
        class="h-full overflow-hidden"
      >
        <CalendarView v-if="showCalendar" />
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from "vue";
import { useRouter } from "vue-router";
import Swal from "sweetalert2";
import {
  UserIcon,
  SettingsIcon,
  LogOutIcon,
  PencilIcon,
  TrashIcon,
  XIcon,
} from "lucide-vue-next";
import CalendarView from "~/components/CalendarView.vue";

const showCalendar = ref(false);
const showNotesSidebar = ref(false);
const showDropdown = ref(false);
const showMainSidebar = ref(false);
const isMobile = ref(false);

definePageMeta({
  middleware: "auth",
});

useHead({
  title: "AcademicHub - Calendário Escolar",
});

const onDateClick = () => {
  showCalendar.value = !showCalendar.value;

  if (isMobile.value) {
    closeMainSidebar();
    closeNotesSidebar();
  }
};

const toggleDropdown = () => {
  showDropdown.value = !showDropdown.value;
};

const currentDate = ref(
  new Date().toLocaleDateString("pt-BR", {
    weekday: "long",
    day: "2-digit",
    month: "long",
    year: "numeric",
  })
);

const openNotesSidebar = () => {
  showNotesSidebar.value = true;
};

const closeNotesSidebar = () => {
  showNotesSidebar.value = false;
};

const openMainSidebar = () => {
  showMainSidebar.value = true;
};

const closeMainSidebar = () => {
  showMainSidebar.value = false;
};

const router = useRouter();
const logout = async () => {
  try {
    await $fetch("/api/logout", { method: "POST" });

    Swal.fire({
      icon: "success",
      title: "Você saiu da sua conta",
      timer: 2000,
      showConfirmButton: false,
    });

    setTimeout(() => router.push("/login"), 1500);
  } catch (error) {
    Swal.fire({
      icon: "error",
      title: "Erro ao sair",
      text: error?.data?.message || "Tente novamente.",
    });
  }
};

const checkIsMobile = () => {
  isMobile.value = window.innerWidth < 768;
};

onMounted(() => {
  checkIsMobile();
  window.addEventListener("resize", checkIsMobile);
});

onUnmounted(() => {
  window.removeEventListener("resize", checkIsMobile);
});
</script>
<style scoped>
.fade-up-enter-active,
.fade-up-leave-active {
  transition: all 0.2s ease;
}
.fade-up-enter-from {
  opacity: 0;
  transform: translateY(10px);
}
.fade-up-enter-to {
  opacity: 1;
  transform: translateY(0);
}
.fade-up-leave-from {
  opacity: 1;
  transform: translateY(0);
}
.fade-up-leave-to {
  opacity: 0;
  transform: translateY(10px);
}
</style>