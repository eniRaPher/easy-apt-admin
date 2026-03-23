<template>
  <div class="max-w-5xl mx-auto">
    <div class="flex items-center space-x-4 mb-6">
      <router-link to="/buildings" class="p-2 bg-white border border-slate-200 rounded-lg hover:bg-slate-50 text-slate-600 transition-colors">
        <ArrowLeft class="w-5 h-5" />
      </router-link>
      <div>
        <h2 class="text-2xl font-bold text-slate-800">ข้อมูลเช่า ห้อง {{ roomNumber }}</h2>
        <p class="text-sm text-slate-500 mt-1">รายละเอียดสัญญาและประวัติการชำระเงิน</p>
      </div>
    </div>

    <div class="grid grid-cols-1 lg:grid-cols-3 gap-6">
      <!-- Rental Info Card -->
      <div class="lg:col-span-1 space-y-6">
        <div v-if="rentalData.status === 'Occupied'" class="bg-white rounded-xl shadow-sm border border-slate-200 p-6 flex flex-col h-full">
          <h3 class="text-lg font-bold text-slate-800 border-b border-slate-100 pb-3 mb-4 flex items-center">
            <UserCheck class="w-5 h-5 mr-2 text-primary-600" />
            ข้อมูลผู้เช่า
          </h3>
          <div class="space-y-4 flex-grow">
            <div>
              <p class="text-xs font-semibold text-slate-500 uppercase tracking-wider mb-1">ชื่อผู้เช่า</p>
              <p class="font-medium text-slate-800">{{ rentalData.tenantName || 'ไม่มีข้อมูล' }}</p>
            </div>
            <div>
              <p class="text-xs font-semibold text-slate-500 uppercase tracking-wider mb-1">วันที่เริ่มเช่า</p>
              <p class="font-medium text-slate-800">{{ rentalData.startDate || '-' }}</p>
            </div>
            <div>
              <p class="text-xs font-semibold text-slate-500 uppercase tracking-wider mb-1">วันสิ้นสุดเช่า</p>
              <p class="font-medium text-slate-800">{{ rentalData.endDate || 'ไม่ระบุ' }}</p>
            </div>
            <div>
              <p class="text-xs font-semibold text-slate-500 uppercase tracking-wider mb-1">สัญญาเช่า (อ้างอิง)</p>
              <div class="flex items-center space-x-2 mt-1">
                <FileText class="w-4 h-4 text-blue-500" />
                <a href="#" class="text-sm font-medium text-blue-600 hover:underline">สัญญาเช่า_{{roomNumber}}.pdf</a>
              </div>
            </div>
          </div>

          <!-- Danger Zone: Move Out Button -->
          <div class="mt-8 pt-5 border-t border-red-100">
             <button @click="isMoveOutModalOpen = true" class="w-full flex items-center justify-center space-x-2 bg-red-50 border border-red-200 text-red-600 hover:bg-red-600 hover:text-white py-2.5 rounded-lg font-bold transition-all shadow-sm group">
               <LogOut class="w-4 h-4 group-hover:animate-pulse" />
               <span>แจ้งย้ายออก (Move Out)</span>
             </button>
             <p class="text-[11px] text-center text-red-500/80 mt-2.5 font-semibold flex items-center justify-center">
               <AlertTriangle class="w-3 h-3 mr-1" /> สิ้นสุดสัญญาเช่าห้อง
             </p>
          </div>
        </div>

        <div v-else-if="rentalData.status === 'Maintenance'" class="bg-white rounded-xl shadow-sm border border-amber-200 p-8 flex flex-col items-center justify-center text-center h-full min-h-[400px]">
          <div class="w-24 h-24 bg-amber-50 text-amber-500 rounded-full flex items-center justify-center mb-6 shadow-sm border border-amber-100">
            <Wrench class="w-12 h-12" />
          </div>
          <h3 class="text-3xl font-black text-slate-800 mb-3">ไม่พร้อมเข้าอยู่</h3>
          <p class="text-slate-500 mb-8 text-sm px-4 leading-relaxed">ห้องนี้กำลังอยู่ระหว่างการซ่อมบำรุง ตรวจสอบ หรือทำความสะอาด ยังไม่พร้อมเปิดให้เช่าสำหรับผู้เช่าใหม่</p>
          <button class="w-full bg-amber-500 hover:bg-amber-600 text-white font-bold py-3.5 rounded-lg shadow-sm flex items-center justify-center transition-colors text-base group cursor-default">
            <Settings class="w-5 h-5 mr-2 group-hover:rotate-90 transition-transform duration-500" />
            ปรับปรุงห้อง
          </button>
        </div>

        <div v-else class="bg-white rounded-xl shadow-sm border border-emerald-200 p-8 flex flex-col items-center justify-center text-center h-full min-h-[400px]">
          <div class="w-24 h-24 bg-emerald-50 text-emerald-500 rounded-full flex items-center justify-center mb-6 shadow-sm border border-emerald-100">
            <CheckCircle class="w-12 h-12" />
          </div>
          <h3 class="text-3xl font-black text-slate-800 mb-3">ห้องว่าง</h3>
          <p class="text-slate-500 mb-8 text-sm px-4 leading-relaxed">ห้องพร้อมเปิดให้เช่า สามารถนำผู้เช่าใหม่เข้าสู่ระบบและผูกสัญญาเช่าได้ทันที</p>
          <button class="w-full bg-emerald-600 hover:bg-emerald-700 text-white font-bold py-3.5 rounded-lg shadow-sm flex items-center justify-center transition-colors text-base">
            <Plus class="w-5 h-5 mr-2" />
            เพิ่มผู้เช่าใหม่
          </button>
        </div>
      </div>

      <!-- Bill History -->
      <div class="lg:col-span-2">
        <div class="bg-white rounded-xl shadow-sm border border-slate-200 overflow-hidden">
          <div class="px-6 py-4 border-b border-slate-100 bg-slate-50 flex justify-between items-center">
            <h3 class="text-lg font-bold text-slate-800 flex items-center">
              <History class="w-5 h-5 mr-2 text-primary-600" />
              ประวัติชำระเงินบิล
            </h3>
            <button @click="showAllBills = !showAllBills" class="text-sm font-medium text-primary-600 hover:text-primary-800 hover:underline transition-colors px-2 py-1 rounded">
              {{ showAllBills ? 'ย่อลง' : 'ดูทั้งหมด' }}
            </button>
          </div>
          
          <div class="overflow-x-auto">
            <table class="w-full text-left text-sm text-slate-600">
              <thead class="bg-slate-50/50 text-slate-500 text-xs uppercase font-semibold">
                <tr>
                  <th class="px-6 py-3">รอบเดือน</th>
                  <th class="px-6 py-3">รหัสบิล</th>
                  <th class="px-6 py-3 text-right">ยอดชำระ</th>
                  <th class="px-6 py-3 text-center">สถานะ</th>
                  <th class="px-6 py-3 text-center">การจัดการ</th>
                </tr>
              </thead>
              <tbody class="divide-y divide-slate-100">
                <tr v-for="bill in displayedBills" :key="bill.id" class="hover:bg-slate-50 transition-colors">
                  <td class="px-6 py-4 font-medium text-slate-800">{{ bill.month }}</td>
                  <td class="px-6 py-4 text-primary-600">{{ bill.code }}</td>
                  <td class="px-6 py-4 text-right font-mono font-medium text-slate-800">฿{{ bill.total.toLocaleString() }}</td>
                  <td class="px-6 py-4 text-center">
                    <span :class="[
                      'px-2.5 py-1 rounded-full text-xs font-medium whitespace-nowrap shadow-sm',
                      bill.status === 'Paid' ? 'bg-green-100 text-green-700' : bill.status === 'Pending' ? 'bg-amber-100 text-amber-700' : 'bg-red-100 text-red-700'
                    ]">
                      {{ bill.status === 'Paid' ? 'ชำระแล้ว' : bill.status === 'Pending' ? 'รอตรวจสอบ' : 'ค้างชำระ' }}
                    </span>
                  </td>
                  <td class="px-6 py-4 text-center space-x-2 whitespace-nowrap">
                    <button @click="openVerifyModal(bill)" class="text-blue-600 hover:text-blue-700 font-bold text-xs transition-colors px-3 py-1.5 bg-blue-50 hover:bg-blue-100 rounded-md border border-blue-200">ตรวจสอบ</button>
                    <button @click="openMessageModal(bill)" class="text-emerald-600 hover:text-emerald-700 font-bold text-xs transition-colors px-3 py-1.5 bg-emerald-50 hover:bg-emerald-100 rounded-md border border-emerald-200">ข้อความ</button>
                  </td>
                </tr>
                <tr v-if="allBills.length === 0">
                  <td colspan="5" class="px-6 py-8 text-center text-slate-500">ไม่มีประวัติบิล</td>
                </tr>
              </tbody>
            </table>
          </div>
        </div>
      </div>
    </div>

    <!-- Modal Move Out -->
    <div v-if="isMoveOutModalOpen" class="fixed inset-0 bg-slate-900/60 backdrop-blur-sm z-50 flex items-center justify-center p-4">
      <div class="bg-white rounded-xl shadow-2xl w-full max-w-md overflow-hidden animate-in fade-in zoom-in-95 duration-200 border-2 border-red-500">
        <div class="p-6 text-center">
          <div class="w-20 h-20 rounded-full bg-red-100 text-red-600 flex items-center justify-center mx-auto mb-5 border-4 border-white shadow-sm">
            <AlertTriangle class="w-10 h-10" />
          </div>
          <h3 class="font-bold text-2xl text-slate-800 mb-2">ยืนยันแจ้งย้ายออก !</h3>
          <p class="text-slate-500 text-sm mb-6">
            คุณกำลังทำเรื่องย้ายออกให้ห้อง <span class="font-bold text-slate-800">{{ roomNumber }}</span><br/>
            การกระทำนี้จะเปลี่ยนสถานะห้องเป็น <span class="text-emerald-600 font-bold">"ว่าง"</span> และลบข้อมูลผู้เช่าปัจจุบัน!
          </p>
          
          <div class="bg-red-50 rounded-lg p-4 mb-2 border border-red-100 text-left">
            <label class="block text-xs font-bold text-red-800 uppercase mb-2">พิมพ์คำว่า "ยืนยันการย้ายออก" เพื่อดำเนินการต่อ</label>
            <input v-model="moveOutConfirmText" type="text" class="w-full border-2 border-red-200 rounded-lg px-3 py-2 text-sm focus:ring-2 focus:ring-red-500 focus:border-red-500 outline-none placeholder-red-300 text-red-900 font-bold" placeholder="พิมพ์ข้อความที่นี่...">
          </div>
        </div>
        
        <div class="px-6 py-4 bg-slate-50 border-t border-slate-100 flex justify-center space-x-3 gap-2">
          <button @click="closeMoveOutModal" class="flex-1 px-4 py-2.5 text-sm font-semibold text-slate-700 bg-white border border-slate-300 rounded-lg hover:bg-slate-50 transition-colors">ยกเลิก</button>
          <button :disabled="moveOutConfirmText !== 'ยืนยันการย้ายออก'" @click="handleMoveOut" class="flex-1 px-4 py-2.5 text-sm font-bold text-white bg-red-600 rounded-lg hover:bg-red-700 transition-colors disabled:opacity-50 disabled:cursor-not-allowed shadow-sm flex justify-center items-center">
            <LogOut class="w-4 h-4 mr-2" /> ย้ายออกทันที
          </button>
        </div>
      </div>
    </div>
    <!-- Verify Payment Modal -->
    <div v-if="isVerifyModalOpen" class="fixed inset-0 bg-slate-900/60 backdrop-blur-sm z-50 flex items-center justify-center p-4">
      <div class="bg-white rounded-xl shadow-2xl w-full max-w-lg overflow-hidden animate-in fade-in zoom-in-95 duration-200">
        <div class="px-6 py-4 border-b border-slate-100 flex justify-between items-center bg-slate-50">
          <h3 class="font-bold text-lg text-slate-800 flex items-center"><FileCheck class="w-5 h-5 mr-2 text-blue-600"/> ตรวจสอบการชำระเงิน</h3>
          <button @click="closeVerifyModal" class="text-slate-400 hover:text-slate-600"><X class="w-5 h-5"/></button>
        </div>
        <div class="p-6">
          <div class="mb-4">
            <h4 class="font-bold text-primary-700 mb-1">บิล: {{ selectedBill?.code }} ({{ selectedBill?.month }})</h4>
            <p class="text-sm text-slate-500">ยอดรวมทั้งสิ้น: <span class="font-bold text-slate-800 text-lg text-primary-600">฿{{ selectedBill?.total.toLocaleString() }}</span></p>
          </div>
          
          <div class="grid grid-cols-2 gap-4 mb-6 text-sm">
             <div class="bg-slate-50 p-3 rounded-lg border border-slate-100">
                <span class="block text-slate-500 mb-1 text-xs uppercase font-bold tracking-wider">ค่าเช่าห้อง</span>
                <span class="font-semibold text-slate-800 border-b border-slate-200 pb-1 block">฿{{ selectedBill?.rent?.toLocaleString() || '4,500' }}</span>
             </div>
             <div class="bg-slate-50 p-3 rounded-lg border border-slate-100">
                <span class="block text-slate-500 mb-1 text-xs uppercase font-bold tracking-wider">ค่าส่วนกลาง</span>
                <span class="font-semibold text-slate-800 border-b border-slate-200 pb-1 block">฿{{ selectedBill?.commonFee?.toLocaleString() || '300' }}</span>
             </div>
             <div class="bg-slate-50 p-3 rounded-lg border border-slate-100">
                <div class="flex justify-between items-center mb-2">
                  <span class="block text-slate-500 text-xs uppercase font-bold tracking-wider">ค่าไฟ ({{ selectedBill?.elecRate }} ฿/ยูนิต)</span>
                </div>
                <div class="flex items-center space-x-2">
                  <input v-if="selectedBill" v-model.number="selectedBill.elecUnits" @input="updateBillTotal" type="number" min="0" class="w-20 border border-amber-300 rounded px-2 py-1.5 text-sm font-bold text-amber-700 bg-white focus:ring-2 focus:ring-amber-500 outline-none text-center shadow-inner">
                  <span class="text-xs text-slate-500 font-medium whitespace-nowrap">ยูนิต</span>
                  <span class="font-semibold text-slate-800 flex-1 text-right">฿{{ ((selectedBill?.elecUnits || 0) * (selectedBill?.elecRate || 0)).toLocaleString() }}</span>
                </div>
             </div>
             <div class="bg-slate-50 p-3 rounded-lg border border-slate-100">
                <div class="flex justify-between items-center mb-2">
                  <span class="block text-slate-500 text-xs uppercase font-bold tracking-wider">ค่าน้ำ ({{ selectedBill?.waterRate }} ฿/ยูนิต)</span>
                </div>
                <div class="flex items-center space-x-2">
                  <input v-if="selectedBill" v-model.number="selectedBill.waterUnits" @input="updateBillTotal" type="number" min="0" class="w-20 border border-blue-300 rounded px-2 py-1.5 text-sm font-bold text-blue-700 bg-white focus:ring-2 focus:ring-blue-500 outline-none text-center shadow-inner">
                  <span class="text-xs text-slate-500 font-medium whitespace-nowrap">ยูนิต</span>
                  <span class="font-semibold text-slate-800 flex-1 text-right">฿{{ ((selectedBill?.waterUnits || 0) * (selectedBill?.waterRate || 0)).toLocaleString() }}</span>
                </div>
             </div>
          </div>

          <div>
             <span class="block text-sm font-semibold text-slate-700 mb-2">สลิปโอนเงินที่แนบมา (แนบโดยผู้เช่า)</span>
             <div class="bg-slate-100 border-2 border-dashed border-slate-300 rounded-xl h-56 flex flex-col items-center justify-center text-slate-400 hover:bg-slate-50 transition-colors group cursor-pointer relative overflow-hidden">
                <img v-if="selectedBill?.status !== 'Pending'" src="https://images.unsplash.com/photo-1621315271772-28b1e3ebfd80?w=500&q=80" alt="slip" class="absolute inset-0 w-full h-full object-cover opacity-20" />
                <div class="text-center group-hover:scale-105 transition-transform relative z-10">
                   <FileCheck class="w-10 h-10 mx-auto mb-2 text-slate-400" />
                   <p class="text-sm font-semibold text-slate-600">สลิปธนาคาร.jpg</p>
                   <p class="text-xs mt-1">คลิกเพื่อดูรูปขนาดเต็ม</p>
                </div>
             </div>
          </div>
        </div>
        <div class="px-6 py-4 bg-slate-50 border-t border-slate-100 flex justify-end space-x-3">
          <button @click="closeVerifyModal" class="px-4 py-2.5 text-sm font-medium text-slate-700 bg-white border border-slate-300 rounded-lg hover:bg-slate-50 transition-colors">ปิด</button>
          <button v-if="selectedBill?.status === 'Pending'" @click="approvePayment" class="px-4 py-2.5 text-sm font-bold text-white bg-emerald-600 rounded-lg hover:bg-emerald-700 flex items-center transition-colors shadow-sm">
            <Check class="w-4 h-4 mr-1.5" /> อนุมัติรับชำระและออกใบเสร็จ
          </button>
        </div>
      </div>
    </div>

    <!-- Message Modal -->
    <div v-if="isMessageModalOpen" class="fixed inset-0 bg-slate-900/60 backdrop-blur-sm z-50 flex items-center justify-center p-4">
      <div class="bg-white rounded-xl shadow-2xl w-full max-w-md overflow-hidden animate-in fade-in zoom-in-95 duration-200 border border-slate-200">
        <div class="px-6 py-4 border-b border-slate-100 flex justify-between items-center bg-slate-50">
          <h3 class="font-bold text-lg text-slate-800 flex items-center"><Send class="w-5 h-5 mr-2 text-emerald-600"/> ส่งข้อความถึงผู้เช่า</h3>
          <button @click="closeMessageModal" class="text-slate-400 hover:text-slate-600"><X class="w-5 h-5"/></button>
        </div>
        <div class="p-6">
          <label class="block text-sm font-semibold text-slate-700 mb-2">ข้อความแจ้งเตือนบิล</label>
          <textarea v-model="messageText" rows="5" class="w-full border border-slate-300 rounded-lg px-3 py-2 text-sm focus:ring-2 focus:ring-primary-500 outline-none resize-none leading-relaxed"></textarea>
          <p class="text-xs text-slate-500 mt-3 flex items-center"><CheckCircle class="w-3 h-3 mr-1 text-green-500" /> แจ้งเตือนจะถูกส่งผ่าน Line Official ทันที</p>
        </div>
        <div class="px-6 py-4 bg-slate-50 border-t border-slate-100 flex justify-end space-x-3">
          <button @click="closeMessageModal" class="px-4 py-2.5 text-sm font-medium text-slate-700 bg-white border border-slate-300 rounded-lg hover:bg-slate-50 transition-colors">ยกเลิก</button>
          <button @click="sendBillMessage" class="px-4 py-2.5 text-sm font-bold text-white bg-primary-600 rounded-lg hover:bg-primary-700 flex items-center transition-colors shadow-sm">
            <Send class="w-4 h-4 mr-1.5" /> ยืนยันการส่งข้อความ
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed, onMounted } from 'vue'
import { useRoute, useRouter } from 'vue-router'
import { ArrowLeft, UserCheck, FileText, History, Users, LogOut, AlertTriangle, CheckCircle, Plus, Wrench, Settings, X, FileCheck, Check, Send } from 'lucide-vue-next'

const route = useRoute()
const router = useRouter()
const roomNumber = ref('')
const showAllBills = ref(false)

const isMoveOutModalOpen = ref(false)
const moveOutConfirmText = ref('')

const closeMoveOutModal = () => {
  isMoveOutModalOpen.value = false
  moveOutConfirmText.value = ''
}

const handleMoveOut = () => {
  if (moveOutConfirmText.value === 'ยืนยันการย้ายออก') {
    alert('✅ บันทึกการย้ายออกเรียบร้อยแล้ว สถานะห้องเปลี่ยนเป็น "ว่าง"')
    closeMoveOutModal()
    router.push('/buildings')
  }
}

// --- Bills Action Modals ---
const isVerifyModalOpen = ref(false)
const selectedBill = ref(null)

const openVerifyModal = (bill) => {
  selectedBill.value = bill
  isVerifyModalOpen.value = true
}
const closeVerifyModal = () => {
  isVerifyModalOpen.value = false
}

const updateBillTotal = () => {
  if (selectedBill.value) {
    const b = selectedBill.value
    b.total = (b.rent || 0) + (b.commonFee || 0) + ((b.elecUnits || 0) * (b.elecRate || 0)) + ((b.waterUnits || 0) * (b.waterRate || 0))
  }
}

const approvePayment = () => {
  if (selectedBill.value) {
    selectedBill.value.status = 'Paid'
  }
  alert(`✅ อนุมัติการชำระเงินบิล ${selectedBill.value.code} เรียบร้อยแล้ว (ระบบได้ออกใบเสร็จส่งให้ผู้เช่าแล้ว)`)
  closeVerifyModal()
}

const isMessageModalOpen = ref(false)
const messageText = ref('')

const openMessageModal = (bill) => {
  selectedBill.value = bill
  messageText.value = `ห้อง ${roomNumber.value} - วันนี้เป็นวันกำหนดชำระบิล ${bill.code} ประจำรอบเดือน ${bill.month} ยอดชำระรวม ฿${bill.total.toLocaleString()} \n\nสามารถชำระเงินโดยการโอนผ่านบัญชีของอพาร์ทเม้นท์ และแนบสลิปเพื่อยืนยันผ่านระบบได้เลยครับ 🏢`
  isMessageModalOpen.value = true
}
const closeMessageModal = () => {
  isMessageModalOpen.value = false
}

const sendBillMessage = () => {
  alert('✅ ระบบได้ส่งข้อความถึงผู้เช่าผ่านทางระบบข้อความหลักและ LINE สำเร็จแล้ว')
  closeMessageModal()
}

// Mock Data
const rentalData = ref({
  status: 'Occupied',
  tenantName: 'สมชาย รักดี',
  startDate: '01 มกราคม 2023',
  endDate: null,
  contractId: 'CT-2023-001'
})

const allBills = ref([
  { id: 1, month: 'ตุลาคม 2023', code: 'INV-10-23', total: 5800, status: 'Pending', rent: 4500, commonFee: 300, elecUnits: 100, elecRate: 8, waterUnits: 10, waterRate: 20 },
  { id: 2, month: 'กันยายน 2023', code: 'INV-09-23', total: 6100, status: 'Paid', rent: 4500, commonFee: 300, elecUnits: 125, elecRate: 8, waterUnits: 15, waterRate: 20 },
  { id: 3, month: 'สิงหาคม 2023', code: 'INV-08-23', total: 5950, status: 'Paid', rent: 4500, commonFee: 300, elecUnits: 110, elecRate: 8, waterUnits: 13.5, waterRate: 20 },
  { id: 4, month: 'กรกฎาคม 2023', code: 'INV-07-23', total: 5800, status: 'Paid', rent: 4500, commonFee: 300, elecUnits: 100, elecRate: 8, waterUnits: 10, waterRate: 20 },
  { id: 5, month: 'มิถุนายน 2023', code: 'INV-06-23', total: 5800, status: 'Paid', rent: 4500, commonFee: 300, elecUnits: 100, elecRate: 8, waterUnits: 10, waterRate: 20 },
  { id: 6, month: 'พฤษภาคม 2023', code: 'INV-05-23', total: 6200, status: 'Paid', rent: 4500, commonFee: 300, elecUnits: 150, elecRate: 8, waterUnits: 10, waterRate: 20 },
  { id: 7, month: 'เมษายน 2023', code: 'INV-04-23', total: 6500, status: 'Paid', rent: 4500, commonFee: 300, elecUnits: 175, elecRate: 8, waterUnits: 15, waterRate: 20 },
  { id: 8, month: 'มีนาคม 2023', code: 'INV-03-23', total: 5800, status: 'Paid', rent: 4500, commonFee: 300, elecUnits: 100, elecRate: 8, waterUnits: 10, waterRate: 20 },
  { id: 9, month: 'กุมภาพันธ์ 2023', code: 'INV-02-23', total: 5900, status: 'Paid', rent: 4500, commonFee: 300, elecUnits: 100, elecRate: 8, waterUnits: 15, waterRate: 20 },
  { id: 10, month: 'มกราคม 2023', code: 'INV-01-23', total: 5800, status: 'Paid', rent: 4500, commonFee: 300, elecUnits: 100, elecRate: 8, waterUnits: 10, waterRate: 20 },
  { id: 11, month: 'ธันวาคม 2022', code: 'INV-12-22', total: 6100, status: 'Paid', rent: 4500, commonFee: 300, elecUnits: 125, elecRate: 8, waterUnits: 15, waterRate: 20 },
  { id: 12, month: 'พฤศจิกายน 2022', code: 'INV-11-22', total: 5800, status: 'Paid', rent: 4500, commonFee: 300, elecUnits: 100, elecRate: 8, waterUnits: 10, waterRate: 20 }
])

const displayedBills = computed(() => {
  return showAllBills.value ? allBills.value : allBills.value.slice(0, 4)
})

const tenantHistory = ref([
  {
    id: 1,
    name: 'สมชาย รักดี',
    phone: '081-234-5678',
    lineId: 'somchai.ruk',
    address: '123/4 หมู่ 5 ต.ช้างเผือก อ.เมือง จ.เชียงใหม่ 50300',
    startDate: '01 มกราคม 2023',
    endDate: null
  },
  {
    id: 2,
    name: 'สมหญิง งามตา',
    phone: '089-876-5432',
    lineId: 'ying_ngam',
    address: '45/6 ถ.สุขุมวิท แขวงคลองเตย เขตคลองเตย กทม. 10110',
    startDate: '15 กุมภาพันธ์ 2022',
    endDate: '31 ธันวาคม 2022'
  }
])

onMounted(() => {
  const id = route.params.id
  roomNumber.value = id
  
  if (id === 'A102') {
    rentalData.value = { status: 'Maintenance', tenantName: null, startDate: null, endDate: null, contractId: null }
    allBills.value = [] // ห้องซ่อมบำรุงไม่มีประวัติบิลแสดง
  } else if (id === 'A201') {
    rentalData.value = { status: 'Vacant', tenantName: null, startDate: null, endDate: null, contractId: null }
    allBills.value = [] // ห้องว่างไม่มีประวัติบิลแสดง
  } else if (id === 'A202') {
    rentalData.value.tenantName = 'มานะ ใจดี'
  } else if (id === 'B101') {
    rentalData.value.tenantName = 'วันชัย งามสุด'
  }
})
</script>
