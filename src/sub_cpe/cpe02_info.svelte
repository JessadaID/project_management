<script>
  import { onMount } from "svelte";
  import { db } from "../firebase";
  import { doc, getDoc } from "firebase/firestore";
  import { Link } from "svelte-routing";

  let documentId = "";
  let items = null; // ตัวแปรสำหรับเก็บข้อมูลที่ดึงมา
  let error = null; // ตัวแปรสำหรับเก็บข้อผิดพลาด
  let loading = true; // สถานะสำหรับแสดงข้อมูลการโหลด

  // ตัวอย่างการใช้ projectId ในการดึงข้อมูลจาก API (สมมุติ)
  onMount(() => {
    // ตัวอย่างการดึงข้อมูลโปรเจคจาก ID
    // คุณสามารถใช้ projectId เพื่อติดต่อ API หรือฐานข้อมูลที่คุณต้องการ
    const urlParts = window.location.pathname.split("/"); // แยกส่วนของ URL
    documentId = urlParts[urlParts.length - 1]; // ดึงค่าหลังสุดของ URL (เช่น 123)
    //console.log(documentId);
  });

  async function fetchDocument() {
    loading = true; // เริ่มโหลด
    try {
      const docRef = doc(db, "project-approve", documentId);
      const docSnap = await getDoc(docRef);

      if (docSnap.exists()) {
        items = { id: docSnap.id, ...docSnap.data() };
        error = null;
      } else {
        error = "Document not found";
        items = null;
      }
    } catch (err) {
      error = err.message;
      items = null;
    } finally {
      loading = false; // สิ้นสุดการโหลด
    }
  }
  $: if (documentId) {
    fetchDocument(); // เรียกใช้เมื่อ documentId มีค่า
  }
</script>

<div class="md:m-5 p-3 md:bg-gray-200 rounded md:shadow-lg relative">
  {#if loading}
  <p>Loading...</p>
{:else if error}
  <p style="color: red;">Error: {error}</p>
{:else if items}
  <div>
    <p> {items.project_name_th}</p>
    <p> {items.project_name_en}</p>
    <p> {items.members}</p>
    <p > {items.adviser}</p>
    <p style="white-space: pre-wrap;"> {items.project_problem}</p>
  </div>
  <Link to={`/cpe_edit/${items.id}`}>
    <button
      class="absolute top-2 right-2 bg-amber-500 text-white px-4 py-2 rounded shadow hover:bg-blue-600"
    >
      แก้ไขข้อมูล
    </button>
  </Link>
{/if}

</div>
