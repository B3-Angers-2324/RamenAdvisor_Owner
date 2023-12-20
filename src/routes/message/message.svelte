<script lang="ts">
    import Template from "../../lib/template.svelte";
    import { API_URL } from "../../main";


    //get id from url
    let url = window.location.href;
    let id = url.substring(url.lastIndexOf('/') + 1);

    //Init the message Array
    let messageData = [];

    //Message API
    fetch(`${API_URL}/message/all/restaurant/${id}?limit=${5}&offset=${0}`,{
            method: "GET",
            headers: {
                "Content-Type": "application/json",
            }
        })
        .then((res) => res.json())
        .then((data) => {
            messageData = data.obj;
            messageData.forEach(element => {
                element.showDropdown = false;
            });
        });
</script>

<Template>
    <h1>Commentaires</h1>

    {#each messageData as msg, i}
        <div id="commentairesContainer">
            <div id="commentaire">
                <div id="left">
                    <div id="title">
                        <h1>{msg.user.firstName} {msg.user.lastName}</h1>
                        <h3>Restaurant {(Math.random() * 2 + 1).toFixed()}</h3>
                    </div>
                    <p>{msg.content}</p>
                </div>
                <div id="right">
                    <p>{msg.note}/5</p>
                </div>
            </div>
        </div>
    {/each}
    {#if messageData.length == 0}
        <p>Aucun commentaire</p>
    {/if}
</Template>


<style lang="scss">
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
                    overflow: auto;
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
    
    h1{
            color: var(--black);
            margin-bottom: 1em;
            padding: var(--spacing) 0 0 var(--spacing);
        }
</style>