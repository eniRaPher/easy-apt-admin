<template>
  <div class="max-w-4xl mx-auto pb-12">
    <div class="mb-8">
      <h2 class="text-2xl font-bold text-slate-800">ตั้งค่าทั่วไป</h2>
      <p class="text-sm text-slate-500 mt-1">จัดการข้อมูลพื้นฐาน ช่องทางการติดต่อ และอัตราค่าบริการของโครงการ</p>
    </div>

    <form @submit.prevent="saveProfile" class="space-y-6">
      
      <!-- Setting Group 1: General Info -->
      <div class="bg-white rounded-xl shadow-sm border border-slate-200 overflow-hidden">
        <div class="p-6 sm:p-8">
          <h3 class="text-lg font-bold text-slate-800 mb-6 border-b border-slate-100 pb-3 flex items-center">
            <Building class="w-5 h-5 mr-2 text-primary-600" />
            ข้อมูลพื้นฐานของโครงการ
          </h3>
          
          <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
            <!-- Project Name -->
            <div class="md:col-span-2">
              <label class="block text-sm font-semibold text-slate-700 mb-2">ชื่อโครงการ / อพาร์ทเม้นท์</label>
              <div class="relative">
                <div class="absolute inset-y-0 left-0 pl-3 flex items-center pointer-events-none">
                  <Building class="h-5 w-5 text-slate-400" />
                </div>
                <input v-model="profileData.projectName" type="text" class="w-full pl-10 border border-slate-300 rounded-lg px-4 py-2.5 text-sm focus:ring-2 focus:ring-primary-500 focus:border-primary-500 outline-none" placeholder="เช่น Easy Apt ลาดพร้าว">
              </div>
            </div>

            <!-- Phone -->
            <div>
              <label class="block text-sm font-semibold text-slate-700 mb-2">เบอร์โทรศัพท์ติดต่อ</label>
              <div class="relative">
                <div class="absolute inset-y-0 left-0 pl-3 flex items-center pointer-events-none">
                  <Phone class="h-5 w-5 text-slate-400" />
                </div>
                <input v-model="profileData.phone" type="tel" class="w-full pl-10 border border-slate-300 rounded-lg px-4 py-2.5 text-sm focus:ring-2 focus:ring-primary-500 focus:border-primary-500 outline-none" placeholder="08x-xxx-xxxx">
              </div>
            </div>

            <!-- Line ID -->
            <div>
              <label class="block text-sm font-semibold text-slate-700 mb-2">LINE ID</label>
              <div class="relative">
                <div class="absolute inset-y-0 left-0 pl-3 flex items-center pointer-events-none">
                  <MessageCircle class="h-5 w-5 text-slate-400" />
                </div>
                <input v-model="profileData.lineId" type="text" class="w-full pl-10 border border-slate-300 rounded-lg px-4 py-2.5 text-sm focus:ring-2 focus:ring-primary-500 focus:border-primary-500 outline-none" placeholder="@easyapt">
              </div>
            </div>

            <!-- Facebook -->
            <div class="md:col-span-2">
              <label class="block text-sm font-semibold text-slate-700 mb-2">ลิงก์ Facebook Page</label>
              <div class="relative">
                <div class="absolute inset-y-0 left-0 pl-3 flex items-center pointer-events-none">
                  <Facebook class="h-5 w-5 text-slate-400" />
                </div>
                <input v-model="profileData.facebook" type="url" class="w-full pl-10 border border-slate-300 rounded-lg px-4 py-2.5 text-sm focus:ring-2 focus:ring-primary-500 focus:border-primary-500 outline-none" placeholder="https://facebook.com/your-page">
              </div>
            </div>

            <!-- Address -->
            <div class="md:col-span-2">
              <label class="block text-sm font-semibold text-slate-700 mb-2">ที่อยู่โครงการ</label>
              <div class="relative">
                <div class="absolute top-3 left-0 pl-3 flex items-start pointer-events-none">
                  <MapPin class="h-5 w-5 text-slate-400" />
                </div>
                <textarea v-model="profileData.address" rows="3" class="w-full pl-10 border border-slate-300 rounded-lg px-4 py-2.5 text-sm focus:ring-2 focus:ring-primary-500 focus:border-primary-500 outline-none resize-none" placeholder="ระบุที่อยู่โครงการ..."></textarea>
              </div>
            </div>
          </div>
        </div>
      </div>

      <!-- Setting Group 2: Rates -->
      <div class="bg-white rounded-xl shadow-sm border border-slate-200 overflow-hidden">
        <div class="p-6 sm:p-8">
          <h3 class="text-lg font-bold text-slate-800 mb-6 border-b border-slate-100 pb-3 flex items-center">
            <Zap class="w-5 h-5 mr-2 text-amber-500" />
            ตั้งค่าอัตราค่าสาธารณูปโภค
          </h3>
          
          <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
            <!-- Electricity Rate -->
            <div>
              <label class="block text-sm font-semibold text-slate-700 mb-2">อัตราค่าไฟฟ้า (บาท/ยูนิต)</label>
              <div class="relative">
                <div class="absolute inset-y-0 left-0 pl-3 flex items-center pointer-events-none">
                  <Zap class="h-5 w-5 text-amber-500" />
                </div>
                <input v-model="profileData.elecRate" type="number" step="0.5" min="0" class="w-full pl-10 border border-slate-300 rounded-lg px-4 py-2.5 text-sm focus:ring-2 focus:ring-amber-500 focus:border-amber-500 outline-none font-mono" placeholder="เช่น 8">
              </div>
            </div>
            <!-- Water Rate -->
            <div>
              <label class="block text-sm font-semibold text-slate-700 mb-2">อัตราค่าน้ำประปา (บาท/ยูนิต)</label>
              <div class="relative">
                <div class="absolute inset-y-0 left-0 pl-3 flex items-center pointer-events-none">
                  <Droplets class="h-5 w-5 text-blue-500" />
                </div>
                <input v-model="profileData.waterRate" type="number" step="0.5" min="0" class="w-full pl-10 border border-slate-300 rounded-lg px-4 py-2.5 text-sm focus:ring-2 focus:ring-blue-500 focus:border-blue-500 outline-none font-mono" placeholder="เช่น 20">
              </div>
            </div>
          </div>
        </div>
      </div>

      <!-- Action Footer -->
      <div class="flex justify-end pt-2">
        <button type="submit" class="bg-primary-600 hover:bg-primary-700 text-white font-bold py-3 px-8 rounded-lg shadow-sm flex items-center transition-colors">
          <Save class="w-5 h-5 mr-2" />
          บันทึกการตั้งค่าทั้งหมด
        </button>
      </div>
          
    </form>
  </div>
</template>

<script setup>
import { ref } from 'vue'
import { Building, Phone, MessageCircle, Facebook, MapPin, Save, Zap, Droplets } from 'lucide-vue-next'

const profileData = ref({
  projectName: 'ระบบจัดการอพาร์ทเม้นท์ Easy Apt',
  phone: '081-234-5678',
  lineId: '@easyapt',
  facebook: 'https://facebook.com/easyapt',
  address: '123/4 หมู่ 5 ต.ช้างเผือก อ.เมือง จ.เชียงใหม่ 50300',
  elecRate: 8,
  waterRate: 20
})

const saveProfile = () => {
  // In a real app, you would dispatch action to store/API
  alert(`บันทึกข้อมูลโปรไฟล์เรียบร้อยแล้ว!\n\nชื่อ: ${profileData.value.projectName}\nติดต่อ: ${profileData.value.phone}`)
}
</script>
