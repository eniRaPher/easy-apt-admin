<template>
  <div class="min-h-screen bg-slate-50 flex flex-col">
    <!-- Top Navbar -->
    <header class="bg-primary-600 shadow-md sticky top-0 z-20">
      <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
        <div class="flex items-center justify-between h-16">
          
          <!-- Logo & Brand -->
          <div class="flex items-center space-x-8">
            <router-link to="/" class="flex-shrink-0 flex items-center space-x-2 hover:opacity-90 transition-opacity">
              <h1 class="text-2xl font-bold text-white tracking-tight">EasyApt</h1>
            </router-link>
            
            <!-- Desktop Navigation -->
            <nav class="hidden md:flex space-x-2">
              <router-link
                v-for="item in menuItems"
                :key="item.path"
                :to="item.path"
                class="flex items-center space-x-2 px-3 py-2 rounded-md text-sm font-medium transition-colors text-primary-50 hover:bg-primary-700 hover:text-white"
                exact-active-class="bg-primary-700 text-white shadow-inner"
              >
                <component :is="item.icon" class="w-4 h-4" />
                <span>{{ item.name }}</span>
              </router-link>
            </nav>
          </div>
          
          <!-- User Profile -->
          <div class="hidden md:block relative">
            <button @click="isProfileMenuOpen = !isProfileMenuOpen" class="flex items-center space-x-3 focus:outline-none hover:opacity-90 transition-opacity">
              <div class="w-9 h-9 rounded-full bg-white text-primary-700 flex items-center justify-center font-bold shadow-sm cursor-pointer">
                A
              </div>
            </button>

            <!-- Overlay for click outside -->
            <div v-if="isProfileMenuOpen" @click="isProfileMenuOpen = false" class="fixed inset-0 z-30"></div>

            <!-- Dropdown Menu -->
            <transition name="fade-scale">
              <div v-if="isProfileMenuOpen" class="absolute right-0 mt-3 w-48 bg-white rounded-lg shadow-lg border border-slate-100 py-1 z-40">
                <router-link to="/profile" @click="isProfileMenuOpen = false" class="px-4 py-2.5 text-sm font-medium text-slate-700 hover:bg-slate-50 transition-colors flex items-center space-x-2">
                  <Settings class="w-4 h-4 text-slate-400" />
                  <span>ตั้งค่าทั่วไป</span>
                </router-link>
                <router-link to="/financial-settings" @click="isProfileMenuOpen = false" class="px-4 py-2.5 text-sm font-medium text-slate-700 hover:bg-slate-50 transition-colors flex items-center space-x-2">
                  <Banknote class="w-4 h-4 text-slate-400" />
                  <span>ตั้งค่าการเงิน</span>
                </router-link>
                <router-link to="/change-password" @click="isProfileMenuOpen = false" class="px-4 py-2.5 text-sm font-medium text-slate-700 hover:bg-slate-50 transition-colors flex items-center space-x-2">
                  <Key class="w-4 h-4 text-slate-400" />
                  <span>เปลี่ยนรหัส</span>
                </router-link>
                <div class="border-t border-slate-100 my-1"></div>
                <button @click="isProfileMenuOpen = false" class="w-full text-left px-4 py-2.5 text-sm font-semibold text-red-600 hover:bg-red-50 transition-colors flex items-center space-x-2">
                  <LogOut class="w-4 h-4 text-red-400" />
                  <span>ออกจากระบบ</span>
                </button>
              </div>
            </transition>
          </div>
          
        </div>
      </div>
    </header>

    <!-- Main Content -->
    <main class="flex-1 w-full max-w-7xl mx-auto p-4 sm:p-6 lg:p-8">
      <router-view v-slot="{ Component }">
        <transition name="fade" mode="out-in">
          <component :is="Component" />
        </transition>
      </router-view>
    </main>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'
import { useRoute } from 'vue-router'
import { LayoutDashboard, Users, Building, Receipt, Send, User, Key, LogOut, Settings, Banknote } from 'lucide-vue-next'

const route = useRoute()
const isProfileMenuOpen = ref(false)

const menuItems = [
  { name: 'แดชบอร์ด', path: '/', icon: LayoutDashboard },
  { name: 'ลูกค้า', path: '/customers', icon: Users },
  { name: 'อาคาร', path: '/buildings', icon: Building },
  { name: 'บิล', path: '/bills', icon: Receipt },
  { name: 'บรอดแคสต์', path: '/messages', icon: Send },
]

const currentRouteName = computed(() => {
  const item = menuItems.find(m => m.path === route.path)
  return item ? item.name : ''
})
</script>

<style>
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.2s ease;
}

.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}

.fade-scale-enter-active,
.fade-scale-leave-active {
  transition: all 0.15s ease-out;
}
.fade-scale-enter-from,
.fade-scale-leave-to {
  opacity: 0;
  transform: scale(0.95) translateY(-5px);
}
</style>
