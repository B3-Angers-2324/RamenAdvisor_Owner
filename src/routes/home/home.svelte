<script>
    import Map from "../../lib/map.svelte";
    import CustomInput from "../../lib/customInput.svelte";

    let showRestaurants = false;
    let showComments = true;
    let showPopup = false;

    function toggleRestaurants(){
        showRestaurants = true;
        showComments = false;
    }

    function toggleComments(){
        showRestaurants = false;
        showComments = true;
    }

    function togglePopup(){
        showPopup = !showPopup;
    }

    let imgUlr = [
        "https://cdn.pixabay.com/photo/2019/02/21/19/00/restaurant-4011989_1280.jpg",
        "https://cdn.pixabay.com/photo/2017/01/26/02/06/platter-2009590_1280.jpg",
        "https://cdn.pixabay.com/photo/2016/11/29/12/54/cafe-1869656_1280.jpg",
        "https://cdn.pixabay.com/photo/2016/11/18/14/39/beans-1834984_1280.jpg"
    ]
</script>

<main>
    {#if showPopup}
        <div id="popup">
            <h1>
                <span class="material-symbols-rounded">delete</span>
                Supprimer
            </h1>
            <p>Êtes vous sûr de vouloir le supprimer</p>
            <div id="btn">
                <button class="material-symbols-rounded" on:click={togglePopup}>delete</button>
                <button class="material-symbols-rounded" on:click={togglePopup}>cancel</button>
            </div>
        </div>
    {/if}
    <!-- La barre de nav à gauche -->
    <div id="nav">
        <!-- Le head -->
        <div id="head">
            <img src="./src/assets/icon.png" alt="logo">
            <h1>RamenAdvisor</h1>
        </div>
        <!-- Le choix des restaurants -->
        <div id="restaurantsContainer">
            <h3>
                <span class="material-symbols-rounded">restaurant</span>
                Mes restaurants
            </h3>
            <!-- la liste des restaurants -->
            <div id="restaurantsList">
                <button class="restaurant" on:click={toggleRestaurants}>
                    Restaurant 1
                    <button class="material-symbols-rounded" on:click|stopPropagation={togglePopup}>delete</button>
                </button>
                <button class="restaurant" on:click={toggleRestaurants}>
                    Restaurant 2
                    <button class="material-symbols-rounded" on:click|stopPropagation={togglePopup}>delete</button>
                </button>
                <button class="restaurant" on:click={toggleRestaurants}>
                    Restaurant 3
                    <button class="material-symbols-rounded" on:click|stopPropagation={togglePopup}>delete</button>
                </button>
                <button class="material-symbols-rounded add" on:click={toggleRestaurants}>add</button>
            </div>
        </div>
        <!-- Le choix des commentaires -->
        <div id="commentairesContainer">
            <h3>
                <span class="material-symbols-rounded">comment</span>
                Mes commentaires
            </h3>
            <!-- la liste des commentaires -->
            <div id="commentairesList">
                <button class="commentaire" on:click={toggleComments}>
                    Restaurant 1
                </button>
                <button class="commentaire" on:click={toggleComments}>
                    Restaurant 2
                </button>
                <button class="commentaire" on:click={toggleComments}>
                    Restaurant 3
                </button>
            </div>
        </div>
        <div id="logout">
            <button>
                <span class="material-symbols-rounded">logout</span>
                Disconnect
            </button>
        </div>
    </div>
    <div id="content">
        {#if showRestaurants}
            <h1>Restaurants</h1>
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
                    <CustomInput title="Nom du restaurant" type="text" required/>
                    <CustomInput title="Adresse" type="text" required/>
                    <CustomInput title="Ville" type="text" required/>
                    <CustomInput title="Numéro de téléphone du restaurant" type="text"/>
                    <CustomInput title="Site internet" type="text"/>
                    <CustomInput title="Mail" type="email"/>
                </div>
                <div id="containerRight">
                    <CustomInput title="Terrasse accessible" type="select">
                        <option value="oui">Oui</option>
                        <option value="non">Non</option>
                    </CustomInput>
                    <CustomInput title="Accès Handicapés" type="select" required>
                        <option value="oui">Oui</option>
                        <option value="non">Non</option>
                    </CustomInput>
                    <p>Menu</p>
                    <!-- <CustomDropzone/> -->
                </div>
            </div>
            <div id="submit">
                <button class="material-symbols-rounded save">save</button>
            </div>
        {/if}
        {#if showComments}
            <h1>Commentaires</h1>

            {#each Array(5) as _, i}
                <div id="commentairesContainer">
                    <div id="commentaire">
                        <div id="left">
                            <div id="title">
                                <h1>User {i+1}</h1>
                                <h3>Restaurant {(Math.random() * 2 + 1).toFixed()}</h3>
                            </div>
                            <p>In deserunt quis proident adipisicing cillum velit incididunt reprehenderit ut dolor elit anim ipsum culpa. Veniam elit ut sint exercitation non sint. Cupidatat sunt dolore deserunt aliqua non.
                                In deserunt quis proident adipisicing cillum velit incididunt reprehenderit ut dolor elit anim ipsum culpa. Veniam elit ut sint exercitation non sint. Cupidatat sunt dolore deserunt aliqua non.
                                In deserunt quis proident adipisicing cillum velit incididunt reprehenderit ut dolor elit anim ipsum culpa. Veniam elit ut sint exercitation non sint. Cupidatat sunt dolore deserunt aliqua non.
                            </p>
                        </div>
                        <div id="right">
                            <p>{(Math.random() * 5).toFixed(1)}/5</p>
                            <button class="material-symbols-rounded" on:click={togglePopup}>delete</button>
                        </div>
                    </div>
                </div>
            {/each}
        {/if}
    </div>
</main>

<style lang="scss">
    main{
        display: flex;
        justify-content: start;
        align-items: center;
        height: 100vh;
        width: 100vw;

        #popup{
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            padding: var(--spacing);
            border-radius: var(--radius);
            z-index: 100000;
            width: fit-content;
            height: fit-content;
            background-color: var(--bone);
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            gap: 1em;

            h1{
                color: var(--white);
                font-size: 2em;
                margin-bottom: 1em;
                display: flex;
                align-items: center;

                span{
                    margin-right: calc(var(--spacing)/2);
                    font-size: 1.5em;
                }
            }

            p{
                color: var(--white);
                font-size: 1.25em;
                margin-bottom: 1em;
            }

            #btn{
                display: flex;
                justify-content: center;
                align-items: center;
                gap: 1em;

                button{
                    padding: calc(var(--spacing) / 2) calc(var(--spacing) * 2);
                    border-radius: var(--radius);
                    border: none;
                    background-color: var(--brunswick-green);
                    color: var(--bone);
                    cursor: pointer;
            
                    &:focus{
                        outline: none;
                    }
                }
            }
        }

        #nav{
            width: 25%;
            height: 100%;
            background-color: var(--bone);
            overflow: scroll;

            #head{
                display: flex;
                flex-direction: column;
                justify-content: center;
                align-items: center;
                height: 20%;
                width: calc(100% - var(--spacing));
                background-color: var(--dark-bone);
                padding: calc(var(--spacing)/2);
                border-bottom: 2px solid var(--black);

                img{
                    width: 7em;
                }

                h1{
                    color: var(--white);
                }
            }

            #restaurantsContainer,
            #commentairesContainer{
                display: flex;
                flex-direction: column;
                justify-content: center;
                width: calc(100% - var(--spacing));
                background-color: var(--dark-bone);
                padding: calc(var(--spacing)/2);
                border-bottom: 2px solid var(--black);

                h3{
                    color: var(--black);
                    margin-bottom: calc(var(--spacing)/2);
                    display: flex;
                    align-items: center;
                    padding-bottom: 1em;

                    span{
                        margin-right: calc(var(--spacing)/2);
                        font-size: 2em;
                    }
                }

                #restaurantsList,
                #commentairesList{
                    display: flex;
                    flex-direction: column;
                    justify-content: center;
                    align-items: center;
                    height: 100%;
                    width: 100%;

                    .restaurant,
                    .commentaire{
                        width: 100%;
                        height: 3em;
                        background-color: var(--brunswick-green);
                        border: none;
                        color: var(--bone);
                        font-size: 1.5em;
                        font-weight: bold;
                        text-align: left;
                        padding: 0 calc(var(--spacing)/2);
                        cursor: pointer;
                        transition: .5s;
                        margin-bottom: 1em;
                        border-radius: var(--radius);
                        display: flex;
                        justify-content: space-between;
                        align-items: center;

                        &:hover{
                            transform: translateX(.5em);
                        }

                        button{
                            background: transparent;
                            border: none;
                            color: var(--bone);
                            font-size: 1.25em;
                            height: 1.5em;
                            width: 1.5em;
                            border-radius: 50%;
                            background-color: rgba(0, 0, 0, 0.15);
                            cursor: pointer;
                        }
                    }

                    .add{
                        width: 100%;
                        height: 2em;
                        background-color: var(--zomp);
                        border: none;
                        color: var(--bone);
                        font-size: 1.5em;
                        font-weight: bold;
                        cursor: pointer;
                        transition: .5s;
                        margin-bottom: 1em;
                        border-radius: var(--radius);
                        text-align: center;
                    }
                }
            }

            #logout{
                display: flex;
                justify-content: center;
                align-items: center;
                width: calc(100% - var(--spacing));
                background-color: var(--dark-bone);
                padding: calc(var(--spacing)/2);

                button{
                    width: 100%;
                    padding: calc(var(--spacing) / 2) 0;
                    background-color: var(--zomp);
                    border: none;
                    color: var(--bone);
                    font-weight: bold;
                    font-size: 1em;
                    cursor: pointer;
                    transition: .5s;
                    border-radius: var(--radius);
                    display: flex;
                    justify-content: center;
                    align-items: center;

                    span{
                        margin-right: calc(var(--spacing)/2);
                        font-size: 1.5em;
                    }
                }
            }
        }

        #content{
            width: 75%;
            height: 100%;
            overflow: scroll;

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

            #commentairesContainer{
                display: flex;
                justify-content: center;
                align-items: center;
                width: 100%;
                height: fit-content;
                margin-bottom: var(--spacing);

                #commentaire{
                    display: flex;
                    justify-content: space-between;
                    align-items: center;
                    width: calc(100% - var(--spacing) * 2);
                    height: 13em;
                    background-color: var(--zomp);
                    border-radius: var(--radius);
                    
                    #left{
                        padding: calc(var(--spacing) / 2);
                        display: flex;
                        flex-direction: column;
                        justify-content: space-between;
                        align-items: flex-start;
                        width: 75%;
                        height: calc(100% - var(--spacing));

                        #title{
                            display: flex;
                            width: 100%;
                            justify-content: space-between;
                            align-items: center;

                            h1,
                            h3{
                                color: var(--black);
                                padding: 0;
                                font-size: 1.5em;
                                font-weight: 600;
                                margin: 0;
                            }
                        }

                        p{
                            color: var(--black);
                            font-size: 1em;
                            max-height: 6em;
                            overflow: scroll;
                        }
                    }

                    #right{
                        position: relative;
                        display: flex;
                        flex-direction: column;
                        justify-content: center;
                        align-items: center;
                        width: 25%;
                        height: 100%;
                        background-color: var(--brunswick-green);
                        border-radius: var(--radius);
                        gap: 1em;

                        p{
                            color: var(--bone);
                            font-size: 1.5em;
                        }

                        button{
                            position: absolute;
                            bottom: calc(var(--spacing) / 2);
                            right: calc(var(--spacing) / 2);
                            background: transparent;
                            border: none;
                            color: var(--bone);
                            font-size: 1.25em;
                            height: 3em;
                            width: 3em;
                            border-radius: 50%;
                            background-color: rgba(0, 0, 0, 0.15);
                            cursor: pointer;
                        }
                    }
                }
            }
        }
    }
</style>