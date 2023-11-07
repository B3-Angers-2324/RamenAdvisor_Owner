<script lang="ts">
  import { Router, Route } from "svelte-routing";
  //Import components routes
  import Register from "./routes/register/register.svelte";
  export let url = "";
</script>


<main>
  <Router {url}>
    <div>
      <Route path="/"><Register /></Route>
    </div>
  </Router> 
</main>
