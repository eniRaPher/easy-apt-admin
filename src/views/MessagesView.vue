<template>
  <div class="max-w-4xl mx-auto">
    <h2 class="text-2xl font-bold text-slate-800 mb-6">ส่งข้อความ (Broadcast)</h2>

    <div class="bg-white rounded-xl shadow-sm border border-slate-200 p-8">
      <form @submit.prevent="sendMessage">
        
        <!-- Broadcast Type -->
        <div class="mb-8">
          <label class="block text-sm font-semibold text-slate-700 mb-4">ช่องทางการส่งข้อความ</label>
          <div class="grid grid-cols-1 md:grid-cols-3 gap-4">
            
            <label class="relative flex cursor-pointer rounded-lg border bg-white p-4 shadow-sm focus:outline-none"
                   :class="broadcastType === 'sms' ? 'border-primary-500 ring-1 ring-primary-500' : 'border-slate-200'">
              <input type="radio" name="broadcast_type" value="sms" v-model="broadcastType" class="sr-only">
              <span class="flex flex-1">
                <span class="flex flex-col">
                  <span class="block text-sm font-medium text-slate-900">SMS</span>
                  <span class="mt-1 flex items-center text-sm text-slate-500">ส่งข้อความไปยังเบอร์มือถือ</span>
                </span>
              </span>
              <MessageSquare class="h-5 w-5 text-primary-600" v-if="broadcastType === 'sms'" />
            </label>

            <label class="relative flex cursor-pointer rounded-lg border bg-white p-4 shadow-sm focus:outline-none"
                   :class="broadcastType === 'line' ? 'border-green-500 ring-1 ring-green-500' : 'border-slate-200'">
              <input type="radio" name="broadcast_type" value="line" v-model="broadcastType" class="sr-only">
              <span class="flex flex-1">
                <span class="flex flex-col">
                  <span class="block text-sm font-medium text-slate-900">LINE Official</span>
                  <span class="mt-1 flex items-center text-sm text-slate-500">ส่งแจ้งเตือนผ่าน LINE</span>
                </span>
              </span>
              <MessageCircle class="h-5 w-5 text-green-500" v-if="broadcastType === 'line'" />
            </label>

            <label class="relative flex cursor-pointer rounded-lg border bg-white p-4 shadow-sm focus:outline-none"
                   :class="broadcastType === 'email' ? 'border-blue-500 ring-1 ring-blue-500' : 'border-slate-200'">
              <input type="radio" name="broadcast_type" value="email" v-model="broadcastType" class="sr-only">
              <span class="flex flex-1">
                <span class="flex flex-col">
                  <span class="block text-sm font-medium text-slate-900">Email</span>
                  <span class="mt-1 flex items-center text-sm text-slate-500">ส่งอีเมลไปยังผู้เช่า</span>
                </span>
              </span>
              <Mail class="h-5 w-5 text-blue-500" v-if="broadcastType === 'email'" />
            </label>
          </div>
        </div>

        <!-- Target Audience -->
        <div class="mb-6">
          <label class="block text-sm font-semibold text-slate-700 mb-2">กลุ่มเป้าหมาย</label>
          <select class="mt-1 block w-full rounded-md border-slate-300 py-3 pl-3 pr-10 text-base focus:border-primary-500 focus:outline-none focus:ring-primary-500 sm:text-sm border">
            <option>ผู้เช่าทั้งหมด</option>
            <option>เฉพาะคนที่ค้างชำระบิล</option>
            <option>เฉพาะอาคาร A</option>
            <option>เฉพาะอาคาร B</option>
          </select>
        </div>

        <!-- Message Template -->
        <div class="mb-8">
          <label class="block text-sm font-semibold text-slate-700 mb-2">ข้อความ (Template)</label>
          <p class="text-xs text-slate-500 mb-3">คุณสามารถใช้ตัวแปร {name}, {room}, {amount} ในข้อความได้</p>
          <textarea
            v-model="messageBody"
            rows="6"
            class="block w-full rounded-md border-slate-300 border shadow-sm focus:border-primary-500 focus:ring-primary-500 sm:text-sm p-4"
            placeholder="เรียนคุณ {name} ห้อง {room}..."
          ></textarea>
        </div>

        <!-- Submit Button -->
        <div class="flex justify-end">
          <button type="button" class="bg-white py-2 px-4 border border-slate-300 rounded-md shadow-sm text-sm font-medium text-slate-700 hover:bg-slate-50 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-primary-500 mr-3">
            ยกเลิก
          </button>
          <button type="submit" class="bg-primary-600 border border-transparent rounded-md shadow-sm py-2 px-6 flex items-center justify-center text-sm font-medium text-white hover:bg-primary-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-primary-500 space-x-2">
            <Send class="w-4 h-4" />
            <span>ส่งข้อความเลย</span>
          </button>
        </div>
      </form>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'
import { MessageSquare, MessageCircle, Mail, Send } from 'lucide-vue-next'

const broadcastType = ref('sms')
const messageBody = ref('เรียนคุณ {name} ห้อง {room},\n\nทางอพาร์ทเมนต์ขอแจ้งเตือนยอดค้างชำระจำนวน {amount} บาท รบกวนชำระเงินภายในวันที่ 5 ของเดือน\n\nขอบคุณค่ะ')

const sendMessage = () => {
  alert(`กำลังส่งข้อความผ่าน ${broadcastType.value} ไปยังผู้เช่า...`)
}
</script>
