<script lang="ts">
  import { Router, Route } from "svelte-routing";
  //Import components routes
  import Register from "./routes/register/register.svelte";
  import Home from "./routes/home/home.svelte";
  import Message from "./routes/message/message.svelte";
  import Restaurant from "./routes/restaurant/restaurant.svelte";
  export let url = "";
</script>


<main>
  <Router {url}>

    <Route path="/"><Register /></Route>
    <Route path="/home"><Home /></Route>
    <Route path="/message"><Home /></Route>
    <Route path="/restaurant"><Home /></Route>
    <Route path="/message/:id"><Message /></Route>
    <Route path="/restaurant/:id"><Restaurant /></Route>

  </Router> 
</main>
