<template>
  <div>
    <h2 class="text-2xl font-bold text-slate-800 mb-6">Dashboard Overview</h2>
    
    <!-- Stats Grid -->
    <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-6 mb-8">
      <!-- Card 1 -->
      <div class="bg-white rounded-xl shadow-sm border border-slate-100 p-6 flex items-center space-x-4">
        <div class="w-12 h-12 rounded-full bg-blue-50 flex items-center justify-center text-blue-600">
          <Building2 class="w-6 h-6" />
        </div>
        <div>
          <p class="text-sm font-medium text-slate-500">ห้องทั้งหมด</p>
          <p class="text-2xl font-bold text-slate-800">120</p>
        </div>
      </div>
      
      <!-- Card 2 -->
      <div class="bg-white rounded-xl shadow-sm border border-slate-100 p-6 flex items-center space-x-4">
        <div class="w-12 h-12 rounded-full bg-green-50 flex items-center justify-center text-green-600">
          <Home class="w-6 h-6" />
        </div>
        <div>
          <p class="text-sm font-medium text-slate-500">ห้องที่มีผู้เช่า (Occupied)</p>
          <p class="text-2xl font-bold text-slate-800">105</p>
        </div>
      </div>
      
      <!-- Card 3 -->
      <div class="bg-white rounded-xl shadow-sm border border-slate-100 p-6 flex items-center space-x-4">
        <div class="w-12 h-12 rounded-full bg-orange-50 flex items-center justify-center text-orange-600">
          <Clock class="w-6 h-6" />
        </div>
        <div>
          <p class="text-sm font-medium text-slate-500">บิลรอชำระ</p>
          <p class="text-2xl font-bold text-slate-800">15</p>
        </div>
      </div>
      
      <!-- Card 4 -->
      <div class="bg-white rounded-xl shadow-sm border border-slate-100 p-6 flex items-center space-x-4">
        <div class="w-12 h-12 rounded-full bg-purple-50 flex items-center justify-center text-purple-600">
          <TrendingUp class="w-6 h-6" />
        </div>
        <div>
          <p class="text-sm font-medium text-slate-500">รายได้เดือนนี้</p>
          <p class="text-2xl font-bold text-slate-800">฿452,000</p>
        </div>
      </div>
    </div>
    
    <!-- Recent Activity Placeholder -->
    <div class="bg-white rounded-xl shadow-sm border border-slate-100 p-6">
      <h3 class="text-lg font-semibold text-slate-800 mb-4">กิจกรรมล่าสุด</h3>
      <div class="space-y-4">
        <div v-for="i in 5" :key="i" class="flex items-center space-x-4 py-3 border-b border-slate-50 last:border-0">
          <div class="w-2 h-2 rounded-full bg-primary-500"></div>
          <div class="flex-1">
            <p class="text-sm font-medium text-slate-700">ชำระค่าเช่าห้อง A10{{ i }} เรียบร้อยแล้ว</p>
            <p class="text-xs text-slate-400">วันนี้, 10:3{{ i }} น.</p>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { Building2, Home, Clock, TrendingUp } from 'lucide-vue-next'
</script>
