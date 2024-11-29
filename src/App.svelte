<script>
  import { Router, Route, Link } from "svelte-routing";
  import HomePage from "./HomePage.svelte";
  
  import Cpe02_wait from "./sub_cpe/cpe02_wait.svelte";
  import Cpe02_home from "./sub_cpe/cpe02_home.svelte";

</script>

<main class="">
 
  <Router>
    <nav class="main-color shadow-md flex text-xl p-5 font-medium">
      <Link to="/">
        <b>โครงงาน/วิจัย</b>
      </Link>
    </nav>
    
    <Route path="/" component={HomePage} />
    <Route path="/cpe" component={Cpe02_home} />
    <Route path="/cpe_wait" component={Cpe02_wait} />
    
  </Router>
</main>
