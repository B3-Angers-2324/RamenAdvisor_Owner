<script lang="ts">
    import Template from "../../lib/template.svelte";
    import CustomInput from "../../lib/customInput.svelte";
    import Map from "../../lib/map.svelte";
    import { API_URL } from "../../main";
    import { onMount } from "svelte";
    import Modal from "../../lib/modal.svelte";

    let mainImage = "";
    let restaurant = {
        "name":"",
        "address":"",
        "city":"Angers",
        "tel":"",
        "web":"",
        "email":"",
        "terrasse":"",
        "accessible":false,
        "foodtype":"",
        "position":undefined,
        "images": []
    };
    let accessible = "non";
    let error = "";
    let imageError = "";
    var foodtypes = {};
    let showModal = false;

    //get id from url
    let url = window.location.href;
    let id = url.substring(url.lastIndexOf('/') + 1);

    onMount(() => {
        getAllFoodTypes();
        getRestaurantInformation();
    })

    const getRestaurantInformation = () => {
        //get restaurant data if id is not "new"
        if(id != "new"){
            fetch(`${API_URL}/restaurant/id/${id}`, {
                    method: 'GET',
                    headers: {
                        'Content-Type': 'application/json',
                    },
                })
                .then((res) => {
                    if(res.status == 500){
                        window.location.href = `/restaurant/new`;
                    }
                    return res.json();
                })
                .then((data) => {
                    Object.keys(restaurant).forEach(key => {
                        restaurant[key] = data.obj[key] || "";
                        if(key == "accessible") accessible = data.obj[key] ? "oui" : "non";
                    });
                    console.log(restaurant);
                    mainImage = restaurant["images"][0];
                })
        }
    }

    //save restaurant
    const handleSaveForm = (e) => {
        e.preventDefault();
        //check if all required fields are filled
        if(restaurant["name"] == "" || restaurant["address"] == "" || restaurant["city"] == "" || accessible == "" || restaurant["foodtype"] == ""){
            error = "Veuillez remplir tous les champs obligatoires";
            return;
        }
        //check if adress is valid
        if(restaurant["position"] == undefined){
            error = "Veuillez entrer une adresse valide";
            return;
        }
        //check if phone number is valid
        if(restaurant["tel"] != "" && restaurant["tel"].length != 10){
            error = "Veuillez entrer un numéro de téléphone valide";
            return;
        }

        restaurant["accessible"] = (accessible == "oui") ? true : false;
        console.log(restaurant);
        if(id == "new"){
            //create new restaurant
            fetch(`${API_URL}/restaurant`, {
                    method: 'POST',
                    headers: {
                        'Content-Type': 'application/json',
                        'Authorization': 'Bearer ' + localStorage.getItem('token')
                    },
                    body: JSON.stringify(restaurant)
                })
                .then((res) => res.json())
                .then((data) => {
                    window.location.href = `/restaurant/${data.id}`;
                })
        }else{
            //update restaurant
            fetch(`${API_URL}/restaurant/id/${id}`, {
                    method: 'PUT',
                    headers: {
                        'Content-Type': 'application/json',
                        'Authorization': 'Bearer ' + localStorage.getItem('token')
                    },
                    body: JSON.stringify(restaurant)
                })
                .then((res) => res.json())
                .then((data) => {
                    error = "Restaurant mis à jour";
                })
        }
    }

    //get position from adress
    const handleExitAdressInput = (e) => {
        fetch(`https://api-adresse.data.gouv.fr/search/?q=${restaurant["address"]+","+restaurant["city"]}&limit=1`)
            .then((res) => res.json())
            .then((data) => {
                if(data.features.length == 0){
                    error = "Veuillez entrer une adresse valide";
                    return;
                }
                restaurant["position"] = [data.features[0].geometry.coordinates[1], data.features[0].geometry.coordinates[0]];
            })
    }

    const getAllFoodTypes = () => {
        foodtypes = {};
        fetch(`${API_URL}/foodtype`, {
                method: 'GET',
                headers: {
                    'Content-Type': 'application/json',
                },
            })
            .then((res) => res.json())
            .then((data) => {
                data.forEach((foodtype) => {
                    foodtypes[foodtype.name] = foodtype.imgId;
                })
            })
    }

    const handleUpdateImage = (e, imageNb) => {
        const file = e.target.files[0];

        // test if the image is a jpg, jpeg or png
        if (file.type !== "image/jpeg" && file.type !== "image/jpg" && file.type !== "image/png") {
            imageError = "L'image doit être au format jpg, jpeg ou png";
            getRestaurantInformation();
            return;
        }

        // test if the image is under 15Mo
        if (file.size > 15000000) {
            imageError = "L'image doit être inférieur à 15Mo";
            getRestaurantInformation();
            return;
        }

        // upload image
        const formData = new FormData();
        formData.append('image', file);
        fetch(`${API_URL}/restaurant/id/${id}/${imageNb}`, {
                method: 'PATCH',
                headers: {
                    'Authorization': 'Bearer ' + localStorage.getItem('token')
                },
                body: formData
            })
            .then((res) => res.json())
            .then((data) => {
                imageError = data.message;
                getRestaurantInformation();
            })
    }

    const handleChangeMainImage = (imageNb) => {
        mainImage = restaurant["images"][imageNb];
    }

    const handleDeleteRestaurant = () => {
        fetch(`${API_URL}/restaurant/id/${id}`, {
                method: 'DELETE',
                headers: {
                    'Authorization': 'Bearer ' + localStorage.getItem('token')
                },
            })
            .then((res) => res.json())
            .then((data) => {
                //redirect to home
                window.location.href = `/home`;
            })
    }
</script>

<Template>
    <h1>{restaurant["name"] || "Ajouter un restaurant"}</h1>
    <div id="containerTop">
        <div id="imagesContainer">
            <div id="mainImg">
                {#if id != "new"}
                    {#if mainImage == "" || mainImage == undefined}
                        <label class="overlay always" for="addImage">
                            <span class="material-symbols-rounded">hide_image</span>
                        </label>
                    {:else}
                        <img src={`${API_URL}/image/${mainImage}`} alt="">
                    {/if}
                {:else}
                    <div class="overlay always">
                        <p>Disponible après la création</p>
                        <p>Du restaurant</p>
                    </div>
                {/if}
                
            </div>
            <div id="sideImgs">
                <!-- for 0 to 3 -->
                {#each Array(3) as _, i}
                    <!-- svelte-ignore a11y-mouse-events-have-key-events -->
                    <!-- svelte-ignore a11y-no-static-element-interactions -->
                    <div class="imageCard" on:mouseover={() => handleChangeMainImage(i)}>
                        {#if id != "new"}
                            <input type="file" id={"addImage"+i} on:change={(e) => handleUpdateImage(e, i)}>
                            {#if restaurant["images"][i] == undefined || restaurant["images"][i] == ""}
                                <label class="overlay always" for={"addImage"+i}>
                                    <span class="material-symbols-rounded">add_a_photo</span>
                                </label>
                            {:else}
                                <img src={`${API_URL}/image/${restaurant["images"][i]}`} alt="">
                                <label class="overlay" for={"addImage"+i}>
                                    <span class="material-symbols-rounded">add_a_photo</span>
                                </label>
                            {/if}
                        {:else}
                            <div class="overlay always disable">
                                <span class="material-symbols-rounded">hide_image</span>
                            </div>
                        {/if}
                    </div>
                {/each}
            </div>
            <p class="error">{imageError}</p>
        </div>
        <div id="map">
            <Map positions={(restaurant["position"])?[restaurant["position"]]:undefined} showPin={true}/>
        </div>
    </div>
    <form on:submit|preventDefault={handleSaveForm}>
        <div id="containerBottom">
            <div id="containerLeft">
                <CustomInput title="Nom du restaurant" type="text" bind:value={restaurant["name"]} required/>
                <CustomInput title="Adresse" type="text" bind:value={restaurant["address"]} handleExitInput={handleExitAdressInput} required/>
                <CustomInput title="Ville" type="text" bind:value={restaurant["city"]} handleExitInput={handleExitAdressInput} required/>
                <CustomInput title="Numéro de téléphone du restaurant" bind:value={restaurant["tel"]} type="tel"/>
                <CustomInput title="Site internet" bind:value={restaurant["web"]} type="text"/>
                <CustomInput title="Mail" bind:value={restaurant["email"]} type="email"/>
            </div>
            <div id="containerRight">
                <CustomInput title="Terrasse accessible" bind:value={restaurant["terrasse"]} type="select">
                    <option value="oui">Oui</option>
                    <option value="non">Non</option>
                </CustomInput>
                <CustomInput title="Accès Handicapés" bind:value={accessible} type="select" required>
                    <option value="oui">Oui</option>
                    <option value="non">Non</option>
                </CustomInput>
                <CustomInput title="Type de nouriture" bind:value={restaurant["foodtype"]} type="select" required>
                    {#each Object.keys(foodtypes) as foodtype}
                        <option value={foodtype}>{foodtype}</option>
                    {/each}
                </CustomInput>
                <p>Menu</p>
                <!-- <CustomDropzone/> -->
            </div>
        </div>
        <p class="error">{error}</p>
        <div id="submit">
            <button type="submit" class="material-symbols-rounded save">save</button>
            
            <button type="button" class="material-symbols-rounded delete" on:click={() => showModal = true}>delete</button>
        </div>
    </form>
    <Modal bind:showModal validate={handleDeleteRestaurant}>
        <h2 slot="header">Voulez-vous vraiment supprimer ce restaurant ?</h2>
    </Modal>
</Template>

<style lang="scss">
    .error{
        color: red;
        text-align: center;
    }

    h1{
        color: var(--black);
        margin-bottom: 1em;
        padding: var(--spacing) 0 0 var(--spacing);
    }

    #containerTop{
        display: flex;
        position: relative;
        justify-content: space-between;
        align-items: flex-start;
        width: calc(100% - var(--spacing) * 2);
        height: 75%;
        margin-bottom: var(--spacing);
        gap: 2em;
        padding-left: var(--spacing);

        #imagesContainer{
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            width: 50%;
            height: 100%;

            #mainImg{
                width: 100%;
                height: 66%;
                margin-bottom: 1em;
                position: relative;
                border-radius: var(--radius);
                overflow: hidden;

                img{
                    width: 100%;
                    height: 100%;
                    object-fit: cover;
                    background-color: rgba(0, 0, 0, 0.15);
                }

                .overlay{
                    position: absolute; 
                    bottom: 0; 
                    background: rgba(0, 0, 0, 0.5); /* Black see-through */
                    width: 100%;
                    height: 100%;
                    transition: .5s ease;
                    opacity: 1;
                    color: white;
                    text-align: center;
                    display: flex;
                    justify-content: center;
                    align-items: center;
                    flex-direction: column;

                    span{
                        font-size: 5em;
                    }
                }
            }

            #sideImgs{
                display: flex;
                justify-content: space-between;
                align-items: center;
                width: 100%;
                height: 33%;                  

                input{
                    width: 0.1px;
                    height: 0.1px;
                    opacity: 0;
                    overflow: hidden;
                    position: absolute;
                    z-index: -1;
                }

                .imageCard{
                    width: 30%;
                    height: 100%;
                    object-fit: cover;
                    border-radius: var(--radius);
                    background-color: rgba(0, 0, 0, 0.15);
                    position: relative;
                    border-radius: var(--radius);
                    overflow: hidden;

                    .overlay{
                        position: absolute; 
                        bottom: 0; 
                        background: rgba(0, 0, 0, 0.5); /* Black see-through */
                        width: 100%;
                        height: 100%;
                        transition: .5s ease;
                        opacity:0;
                        color: white;
                        font-size: 20px;
                        text-align: center;
                        display: flex;
                        justify-content: center;
                        align-items: center;
                        cursor: pointer;

                        &.always{
                            opacity: 1;
                        }

                        &.disable{
                            cursor: not-allowed;
                        }
                    }

                    img{
                        width: 100%;
                        height: 100%;
                        object-fit: cover;
                        background-color: rgba(0, 0, 0, 0.15);  
                    }

                    &:hover{
                        .overlay{
                            opacity: 1;
                        }
                    }
                }
            }
        }

        #map{
            width: 50%;
            height: 100%;
            background-color: var(--dark-bone);
            border-radius: var(--radius);
        }
    }

    #containerBottom{
        display: flex;
        justify-content: space-between;
        align-items: flex-start;
        width: calc(100% - var(--spacing) * 2);
        height: fit-content;
        gap: 2em;
        padding-left: var(--spacing);

        #containerLeft,
        #containerRight{
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: flex-start;
            width: 50%;
            height: 100%;
            gap: 1em;
            
            #customInput{
                width: 100%;
            }
        }
    }

    #submit{
        width: 100%;
        display: flex;
        justify-content: center;
        align-items: center;
        margin: var(--spacing) 0;

        .save, .delete{
            position: relative;
            cursor: pointer;
            padding: calc(var(--spacing) / 2) calc(var(--spacing) * 2);
            border-radius: var(--radius);
            border: none;
            color: var(--bone);
    
            &:focus{
                outline: none;
            }
        }

        .save{
            background-color: var(--brunswick-green);
        }
        .delete{
            margin-left: 2em;
            background-color: var(--danger);
        }


    }
</style>