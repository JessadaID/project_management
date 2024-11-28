<script>
  import { onMount } from "svelte";
  import { collection, getDocs } from "firebase/firestore";
  import { db } from "../firebase";
  import { Link } from "svelte-routing";
  let items = []; // ข้อมูลโปรเจคทั้งหมด
  let filteredItems = []; // ข้อมูลที่ผ่านการกรอง
  let searchQuery = ""; // ตัวแปรสำหรับเก็บค่าการค้นหา
  let loading = true; // สถานะการโหลดข้อมูล
  let error = null; // เก็บข้อผิดพลาดถ้ามี
  let project_status = "all"; // สถานะโปรเจค

  // ดึงข้อมูลทั้งหมดจาก Firestore
  onMount(async () => {
    try {
      const querySnapshot = await getDocs(collection(db, "project-approve"));
      items = querySnapshot.docs.map((doc) => ({ id: doc.id, ...doc.data() }));
    } catch (err) {
      error = "เกิดข้อผิดพลาดในการโหลดข้อมูล: " + err.message;
    } finally {
      loading = false;
    }
  });

  // กรองข้อมูลตาม searchQuery และ project_status
  $: filteredItems = items
    .filter(
      (item) => project_status === "all" || item.status === project_status
    )
    .filter((item) =>
      item.project_name_th.toLowerCase().includes(searchQuery.toLowerCase())
    );
</script>

<div class="p-5">
  <p>ค้นหาข้อมูล</p>
  <!-- Input การค้นหา -->
  <input
    type="text"
    bind:value={searchQuery}
    placeholder="ค้นหาโปรเจคตามชื่อ..."
    class="w-full p-2 border border-gray-300 rounded mb-4"
  />
  <!-- เลือกสถานะ -->
  <div class="flex justify-end mb-4">
    <p class="p-2 font-bold">สถานะ : &nbsp;</p>
    <select
      bind:value={project_status}
      class="p-2 border border-gray-300 rounded"
    >
      <option value="all">แสดงทั้งหมด</option>
      <option value="wait">รออนุมัติ</option>
      <option value="improvement">แก้ไข</option>
      <option value="approve">อนุมัติ</option>
    </select>
  </div>
  <!-- Loading, Error หรือแสดงรายการโปรเจค -->
  {#if loading}
    <p>กำลังโหลดข้อมูล...</p>
  {:else if error}
    <p class="text-red-500">{error}</p>
  {:else if filteredItems.length > 0}
    {#each filteredItems as item}
      <div class="md:m-5 mb-5 p-3 bg-gray-200 rounded shadow-lg relative">
        <ul>
          <li class="text-xl mt-3">
            <b>ชื่อโครงงาน</b><br /> &emsp;&emsp;{item.project_name_th}
          </li>
          <li class="mt-2 text-xl">
            <b>ชื่อผู้เสนอโครงงาน</b><br />
            {#each item.members as members}
              &emsp;&emsp;{members}<br />
            {/each}
          </li>
          <li class="mt-2 text-xl">
            <b>อาจารย์ที่ปรึกษาโครงงาน</b><br />
            {#each item.adviser as adviser}
              &emsp;&emsp;{adviser}<br />
            {/each}
          </li>
          <li class="mt-2 text-xl flex">
            {#if item.status == "wait"}
              <b>สถานะ : &nbsp;</b>
              <p class="text-sky-500 font-bold flex">
                รอการอนุมัติ&nbsp;<svg
                  xmlns="http://www.w3.org/2000/svg"
                  fill="none"
                  viewBox="0 0 24 24"
                  stroke-width="1.5"
                  stroke="currentColor"
                  class="size-6"
                >
                  <path
                    stroke-linecap="round"
                    stroke-linejoin="round"
                    d="M12 6v6h4.5m4.5 0a9 9 0 1 1-18 0 9 9 0 0 1 18 0Z"
                  />
                </svg>
              </p>
            {:else if item.status == "improvement"}
              <b>สถานะ : &nbsp;</b>
              <p class="text-amber-500 font-bold flex">
                แก้ไขเอกสาร&nbsp; <svg
                  xmlns="http://www.w3.org/2000/svg"
                  fill="none"
                  viewBox="0 0 24 24"
                  stroke-width="1.5"
                  stroke="currentColor"
                  class="size-6"
                >
                  <path
                    stroke-linecap="round"
                    stroke-linejoin="round"
                    d="M12 9v3.75m-9.303 3.376c-.866 1.5.217 3.374 1.948 3.374h14.71c1.73 0 2.813-1.874 1.948-3.374L13.949 3.378c-.866-1.5-3.032-1.5-3.898 0L2.697 16.126ZM12 15.75h.007v.008H12v-.008Z"
                  />
                </svg>
              </p>
            {:else if item.status == "approve"}
              <b>สถานะ : &nbsp;</b>
              <p class="text-green-500 font-bold flex">
                อนุมัติแล้ว&nbsp;<svg
                  xmlns="http://www.w3.org/2000/svg"
                  fill="none"
                  viewBox="0 0 24 24"
                  stroke-width="1.5"
                  stroke="currentColor"
                  class="size-6"
                >
                  <path
                    stroke-linecap="round"
                    stroke-linejoin="round"
                    d="M9 12.75 11.25 15 15 9.75M21 12a9 9 0 1 1-18 0 9 9 0 0 1 18 0Z"
                  />
                </svg>
              </p>
            {/if}
          </li>
        </ul>
        <Link to={`/cpe_wait/${item.id}`}>
          <button
            class="absolute top-2 right-2 bg-blue-500 text-white px-4 py-2 rounded shadow hover:bg-blue-600"
          >
            แสดงเพิ่มเติม
          </button>
        </Link>
      </div>
    {/each}
  {:else}
    <p>ไม่พบข้อมูลที่ตรงกับคำค้นหา</p>
  {/if}
</div>

<style>
  input {
    font-size: 16px;
  }
  p {
    font-size: 18px;
  }
</style>
