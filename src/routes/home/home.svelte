<script lang="ts">
    import Template from "../../lib/template.svelte";
    import { API_URL } from "../../main";

    fetch(`${API_URL}/owner/restaurants`, {
            method: 'GET',
            headers: {
                'Content-Type': 'application/json',
                'Authorization': 'Bearer ' + localStorage.getItem('token')
            },
        })
        .then((res) => res.json())
        .then((data) => {
            if(data.restaurants.length > 0){
                window.location.href = `/restaurant/${data.restaurants[0]._id}`;
            }else{
                window.location.href = `/restaurant/new`;
            }
        })
</script>

<Template>
</Template>

<style lang="scss">
</style>