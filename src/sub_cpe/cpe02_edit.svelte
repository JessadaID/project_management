<script>
    import { db } from '../firebase'; // นำเข้า Firebase Config
    import { doc, setDoc } from "firebase/firestore"; // ใช้ setDoc หรือ updateDoc
  
    let items = {};
    let members = [""];
    // ดึงข้อมูล JSON และแปลงกลับเป็น Object
    items = JSON.parse(localStorage.getItem("documentData") || "{}");
    $: localStorage.setItem("documentData", JSON.stringify(items));
  
  
    async function handleSubmit(event) {
      event.preventDefault();
  
      try {
        // อัปเดตข้อมูลใน Firestore
        const docRef = doc(db, "project-approve", items.id); // ระบุ collection และ document ID
        await setDoc(docRef, items);
  
        alert("ข้อมูลถูกอัปเดตใน Firebase เรียบร้อยแล้ว!");
      } catch (error) {
        console.error("Error updating document: ", error);
        alert("เกิดข้อผิดพลาดในการอัปเดตข้อมูล");
      }
    }
  
  
    function handleTab(event) {
      if (event.key === "Tab") {
        event.preventDefault();
        const textarea = event.target;
        const start = textarea.selectionStart || 0;
        const end = textarea.selectionEnd || 0;
  
        // เพิ่ม Tab ที่ตำแหน่งที่กำลังพิมพ์
        textarea.value =
          textarea.value.substring(0, start) +
          "\t" +
          textarea.value.substring(end);
  
        // ปรับตำแหน่ง cursor
        textarea.selectionStart = textarea.selectionEnd = start + 1;
  
        // อัปเดตค่าที่ bind
        items.project_problem = textarea.value;
      }
    }
  
    function addMemberRow() {
      items.members = [...items.members, ""]; // เพิ่มสมาชิกใหม่ใน array
    }
    
    // ฟังก์ชันในการลบแถวล่างสุด
    function deleteLastMember() {
      if (items.members.length > 1) {
          items.members = items.members.slice(0, -1); // ลบสมาชิกแถวสุดท้าย
      }
    }
  </script>
  
  <div class="md:m-5 md:p-5 flex justify-center items-center">
    <form
      class="shadow-lg p-5 md:rounded-lg md:w-8/12 third-color"
      on:submit={handleSubmit}
    >
      <div class="p-5">
        <!--===============================================-->
  
        <label for="name" class="block text-lg font-medium">ภาคเรียน</label>
        <select id="dropdown" name="term" class="p-2 w-4/12" bind:value={items.term}>
          <option value="2/2567" selected>2/2567</option>
          <option value="1/2568">1/2568</option>
          <option value="2/2568">2/2568</option>
        </select>
  
        <!--===============================================-->
  
        <label for="text" class="block text-lg font-medium mt-3"
          >ชื่อโครงงาน (ภาษาไทย)</label
        >
        <input
          type="text"
          placeholder="ชื่อโครงงาน"
          name="project_name"
          required
          class="p-2 border border-gray-300 rounded focus:outline-none focus:ring-2 focus:ring-blue-500 w-full"
          bind:value={items.project_name_th}
        />
  
        <!--===============================================-->
  
        <label for="text" class="block text-lg font-medium mt-3"
          >ชื่อโครงงาน (ภาษาอังกฤษ)</label
        >
        <input
          type="text"
          placeholder="Name Project"
          required
          class="p-2 border border-gray-300 rounded focus:outline-none focus:ring-2 focus:ring-blue-500 w-full"
          bind:value={items.project_name_en}
        />
  
        <!--===============================================-->
        <label for="text" class="block text-lg font-medium mt-3"
          >ชื่อผู้เสนอโครงงาน
        </label>
        <button
          type="button"
          class="bg-white p-1 m-2 rounded"
          on:click={addMemberRow}>เพิ่มสมาชิก</button
        >
        <button
          type="button"
          on:click={deleteLastMember}
          class="bg-red-500 text-white p-1 m-2 rounded"
        >
          ลบสมาชิก
        </button>
        <div class="input-row grid md:grid-cols-2 gap-4">
          {#each items.members as member, index}
            <input
              type="text"
              bind:value={items.members[index]}
              placeholder="Member Name"
              class="p-2 border border-gray-300 rounded focus:outline-none focus:ring-2 focus:ring-blue-500 m-1"
            />
          {/each}
        </div>
  
        <!--===============================================-->
  
        <label for="email" class="block text-lg font-medium mt-3"
          >อาจารย์ที่ปรึกษาโครงงาน
        </label>
        <select
          id="dropdown"
          name="adviser"
          class="p-2 w-full"
          multiple
          size="4"
          bind:value={items.adviser}
        >
          <option value="ผู้ช่วยศาสตราจารย์ อนัน ทับเกิด"
            >ผู้ช่วยศาสตราจารย์ อนัน ทับเกิด</option
          >
          <option value="นายกิตตินันท์ น้อยมณี">นายกิตตินันท์ น้อยมณี</option>
          <option value="ผู้ช่วยศาสตราจารย์ ขวัญชัย เอื้อวิริยานุกูล"
            >ผู้ช่วยศาสตราจารย์ ขวัญชัย เอื้อวิริยานุกูล</option
          >
          <option value="นายจักรภพ ใหม่เสน">นายจักรภพ ใหม่เสน</option>
          <option value="5นายณัฐชาสิทธิ์ ชูเกียรติขจร"
            >นายณัฐชาสิทธิ์ ชูเกียรติขจร</option
          >
          <option value="นายปณต พุกกะพันธุ์">นายปณต พุกกะพันธุ์</option>
          <option value="นายปิยพล ยืนยงสถาวร">นายปิยพล ยืนยงสถาวร</option>
          <option value="นายพิชิต ทนันชัย">นายพิชิต ทนันชัย</option>
          <option value="นางสาวยุพดี หัตถสิน">นางสาวยุพดี หัตถสิน</option>
          <option value="นายสมนึก สุระธง">นายสมนึก สุระธง</option>
          <option value="นายภาณุเดช ทิพย์อักษร">นายภาณุเดช ทิพย์อักษร</option>
          <option value="นายอนุพงศ์ ไพโรจน์">นายอนุพงศ์ ไพโรจน์</option>
          <option value="นายอรรถพล วิเวก">นายอรรถพล วิเวก</option>
        </select>
        <!--===============================================-->
  
        <label for="text" class="block text-lg font-medium mt-3"
          >ที่มาและความสำคัญของปัญหา
        </label>
  
        <textarea
          id="editor"
          name="w3review"
          rows="10"
          cols="50"
          class="w-full"
          bind:value={items.project_problem}
          on:keydown={handleTab}
          placeholder="เขียนที่มาและความสำคัญของปัญหา..."
        ></textarea>
      </div>
      <!--===============================================-->
  
      <div class="p-5 text-center">
        <button
          type="submit"
          class="rounded bg-blue-500 hover:bg-blue-700 text-white px-6 py-2 shadow-md w-full"
        >
          แก้ไขข้อมูล
        </button>
      </div>
  
      <!--===============================================-->
    </form>
  </div>