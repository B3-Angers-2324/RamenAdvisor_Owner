<script lang="ts">
    import Middleware from "../../routes/middleware/middleware.svelte";
    import Template from "../../lib/template.svelte";
    import CustomInput from "../../lib/customInput.svelte";
    import Map from "../../lib/map.svelte";
    import { API_URL } from "../../main";

    let imgUlr = [
        "https://cdn.pixabay.com/photo/2019/02/21/19/00/restaurant-4011989_1280.jpg",
        "https://cdn.pixabay.com/photo/2017/01/26/02/06/platter-2009590_1280.jpg",
        "https://cdn.pixabay.com/photo/2016/11/29/12/54/cafe-1869656_1280.jpg",
        "https://cdn.pixabay.com/photo/2016/11/18/14/39/beans-1834984_1280.jpg"
    ]
    let restaurant = [];

    //get id from url
    let url = window.location.href;
    let id = url.substring(url.lastIndexOf('/') + 1);

    fetch(`${API_URL}/restaurant/id/${id}`, {
            method: 'GET',
            headers: {
                'Content-Type': 'application/json',
            },
        })
        .then((res) => res.json())
        .then((data) => {
            restaurant = data.obj;
            console.log(restaurant);
        })
</script>

<Template>
    <Middleware>
        <h1>{restaurant["name"]}</h1>
        <div id="containerTop">
            <div id="imagesContainer">
                <div id="mainImg">
                    <img src="{imgUlr[0]}" alt="">
                </div>
                <div id="sideImgs">
                    <img src="{imgUlr[1]}" alt="">
                    <img src="{imgUlr[2]}" alt="">
                    <img src="{imgUlr[3]}" alt="">
                </div>
            </div>
            <div id="map">
                <Map positions={[[47.47908262135688, -0.5540509578361477]]}/>
            </div>
        </div>
        <div id="containerBottom">
            <div id="containerLeft">
                <CustomInput title="Nom du restaurant" type="text" value={restaurant["name"] || ""} required/>
                <CustomInput title="Adresse" type="text" value={restaurant["address"] || ""} required/>
                <CustomInput title="Ville" type="text" value="Angers" required/>
                <CustomInput title="Numéro de téléphone du restaurant" value={restaurant["tel"] || ""} type="text"/>
                <CustomInput title="Site internet" value={restaurant["web"] || ""} type="text"/>
                <CustomInput title="Mail" value={restaurant["email"] || ""} type="email"/>
            </div>
            <div id="containerRight">
                <CustomInput title="Terrasse accessible" value={restaurant["terrasse"] || ""} type="select">
                    <option value="oui">Oui</option>
                    <option value="non">Non</option>
                </CustomInput>
                <CustomInput title="Accès Handicapés" value={restaurant["handicap"] || ""} type="select" required>
                    <option value="oui">Oui</option>
                    <option value="non">Non</option>
                </CustomInput>
                <CustomInput title="Type de nouriture" value={restaurant["foodtype"] || ""} type="select">
                    <option value="asiatique">Asiatique</option>
                    <option value="burger">Burger</option>
                    <option value="francais">Français</option>
                    <option value="italien">Italien</option>
                    <option value="japonais">Japonais</option>
                    <option value="kebab">Kebab</option>
                    <option value="libanais">Libanais</option>
                    <option value="marocain">Marocain</option>
                    <option value="pizzeria">Pizzeria</option>
                    <option value="salade">Salade</option>
                    <option value="sandwich">Sandwich</option>
                    <option value="tapas">Tapas</option>
                    <option value="thai">Thaï</option>
                    <option value="vietnamien">Vietnamien</option>
                    <option value="local_pizza">Pizza Local</option>
                    <option value="lunch_dining">Lunch</option>
                </CustomInput>
                <p>Menu</p>
                <!-- <CustomDropzone/> -->
            </div>
        </div>
        <div id="submit">
            <button class="material-symbols-rounded save">save</button>
        </div>
    </Middleware>
</Template>

<style lang="scss">
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

                img{
                    width: 100%;
                    height: 100%;
                    object-fit: cover;
                    border-radius: var(--radius);
                    background-color: rgba(0, 0, 0, 0.15);
                }
            }

            #sideImgs{
                display: flex;
                justify-content: space-between;
                align-items: center;
                width: 100%;
                height: 33%;

                img{
                    width: 30%;
                    height: 100%;
                    object-fit: cover;
                    border-radius: var(--radius);
                    background-color: rgba(0, 0, 0, 0.15);
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

        .save{
            position: relative;
            cursor: pointer;
            padding: calc(var(--spacing) / 2) calc(var(--spacing) * 2);
            border-radius: var(--radius);
            border: none;
            background-color: var(--brunswick-green);
            color: var(--bone);
    
            &:focus{
                outline: none;
            }
        }
    }
</style>