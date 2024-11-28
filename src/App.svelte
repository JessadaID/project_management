<script>
  import { Router, Route, Link } from "svelte-routing";
  import HomePage from "./HomePage.svelte";
  
  import Cpe02_form from "./sub_cpe/cpe02_form.svelte";
  import Cpe02_wait from "./sub_cpe/cpe02_wait.svelte";
  import Cpe02_home from "./sub_cpe/cpe02_home.svelte";
  import Cpe02_info from "./sub_cpe/cpe02_info.svelte";
  import Cpe02_edit from "./sub_cpe/cpe02_edit.svelte";
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
    <Route path="/cpe_form" component={Cpe02_form} />
    <Route path="/cpe_wait" component={Cpe02_wait} />
    <Route path="/cpe_wait/:id" component={Cpe02_info} />
    <Route path="/cpe_edit/:id" component={Cpe02_edit} />
  </Router>
</main>
