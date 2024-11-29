<script>
  import { Link } from "svelte-routing";
</script>

<main>
  <div class="grid md:grid-cols-2 gap-4 m-5 rounded">
    <Link to="/cpe_form">
      <div class="hover:bg-red-200 p-5 border-gray-500 border-2 bg-white">
        <b>กรอกแบบฟอร์ม CPE02</b>
      </div>
    </Link>
  
    <Link to="/cpe_wait">
      <div class="hover:bg-red-200 p-5 border-gray-500 border-2 bg-white">
        <b>ข้อมูลใบเสนอโปรเจคทั้งหมด</b>
      </div>
   
  </div>
</main>
