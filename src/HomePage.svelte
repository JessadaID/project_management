<script>
  import "./app.css";
  import { Link } from "svelte-routing";
</script>

<main>
  <div class="grid md:grid-cols-6 gap-4 m-5 rounded auto-rows-[1fr]">
    <Link to="/cpe">
      <div
        class="hover:bg-red-200 p-5 border-gray-500 border-2 bg-white aspect-w-1 aspect-h-1 flex items-center justify-center"
      >
        <b>ใบเสนอโครงงาน CPE02</b>
      </div>
    </Link>
   
  </div>
</main>
