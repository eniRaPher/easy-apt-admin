<template>
  <div>
    <div class="flex items-center justify-between mb-6">
      <h2 class="text-2xl font-bold text-slate-800">บิล (Bills)</h2>
      <div class="flex space-x-3">
        <button class="bg-white border border-slate-200 text-slate-700 px-4 py-2 rounded-lg font-medium shadow-sm transition-colors hover:bg-slate-50 flex items-center space-x-2">
          <Download class="w-4 h-4" />
          <span>ส่งออก (Export)</span>
        </button>
        <button class="bg-primary-600 hover:bg-primary-700 text-white px-4 py-2 rounded-lg font-medium shadow-sm transition-colors flex items-center space-x-2">
          <Plus class="w-4 h-4" />
          <span>สร้างบิลใหม่</span>
        </button>
      </div>
    </div>

    <!-- Table Container -->
    <div class="bg-white rounded-xl shadow-sm border border-slate-200 overflow-hidden">
      <div class="overflow-x-auto">
        <table class="w-full text-left text-sm text-slate-600">
          <thead class="bg-slate-50 text-slate-700 uppercase font-semibold border-b border-slate-200">
            <tr>
              <th class="px-6 py-4">รหัส (Code)</th>
              <th class="px-6 py-4">ห้อง</th>
              <th class="px-6 py-4">วันที่สร้าง</th>
              <th class="px-6 py-4 text-right">ค่าเช่า</th>
              <th class="px-6 py-4 text-right">ค่ามิเตอร์ (ไฟ)</th>
              <th class="px-6 py-4 text-right">ค่าน้ำ</th>
              <th class="px-6 py-4 text-right">ค่าส่วนกลาง</th>
              <th class="px-6 py-4 text-right">ยอดรวม</th>
              <th class="px-6 py-4">สถานะ</th>
            </tr>
          </thead>
          <tbody class="divide-y divide-slate-100">
            <tr v-for="bill in bills" :key="bill.id" class="hover:bg-slate-50 transition-colors">
              <td class="px-6 py-4 font-medium text-primary-600">{{ bill.code }}</td>
              <td class="px-6 py-4 font-bold text-slate-800">{{ bill.room }}</td>
              <td class="px-6 py-4">{{ bill.createdAt }}</td>
              <td class="px-6 py-4 text-right">฿{{ bill.rent.toLocaleString() }}</td>
              <td class="px-6 py-4 text-right">฿{{ bill.electric.toLocaleString() }}</td>
              <td class="px-6 py-4 text-right">฿{{ bill.water.toLocaleString() }}</td>
              <td class="px-6 py-4 text-right">฿{{ bill.common.toLocaleString() }}</td>
              <td class="px-6 py-4 text-right font-bold text-slate-800">฿{{ bill.total.toLocaleString() }}</td>
              <td class="px-6 py-4">
                <span :class="[
                  'px-2.5 py-1 rounded-full text-xs font-medium',
                  bill.status === 'Paid' ? 'bg-green-100 text-green-700' : 'bg-orange-100 text-orange-700'
                ]">
                  {{ bill.status === 'Paid' ? 'ชำระแล้ว' : 'รอชำระ' }}
                </span>
              </td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'
import { Plus, Download } from 'lucide-vue-next'

const bills = ref([
  { id: 1, code: 'INV-202310-001', room: 'A101', createdAt: '01 ต.ค. 2023', rent: 4500, electric: 850, water: 150, common: 300, total: 5800, status: 'Paid' },
  { id: 2, code: 'INV-202310-002', room: 'A102', createdAt: '01 ต.ค. 2023', rent: 4500, electric: 620, water: 150, common: 300, total: 5570, status: 'Pending' },
  { id: 3, code: 'INV-202310-003', room: 'B205', createdAt: '01 ต.ค. 2023', rent: 5000, electric: 1200, water: 200, common: 300, total: 6700, status: 'Paid' },
  { id: 4, code: 'INV-202310-004', room: 'C301', createdAt: '01 ต.ค. 2023', rent: 4000, electric: 450, water: 100, common: 300, total: 4850, status: 'Pending' },
])
</script>
