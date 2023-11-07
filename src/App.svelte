<script lang="ts">
  import { Router, Route } from "svelte-routing";
  //Import components routes
  import Register from "./routes/register/register.svelte";
  import Home from "./routes/home/home.svelte";
  export let url = "";
</script>


<main>
  <Router {url}>
    <div>
      <Route path="/"><Register /></Route>
      <Route path="/home"><Home /></Route>
    </div>
  </Router> 
</main>
