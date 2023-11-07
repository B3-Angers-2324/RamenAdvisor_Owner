<script>
    import CustomInput from "./customInput.svelte";

    let signIn = true;
    let signUp = false;

    let toggle = () => {
        window.scrollTo(0, 0);
        signIn = !signIn;
        signUp = !signUp;
    }

</script>

<main>
    <div id="container">
        {#if signIn}
            <h1>Sign In</h1>
            <form>
                <CustomInput type="email" title="Email" required />
                <CustomInput type="password" title="Password" required />
                <CustomInput type="submit">Sign In</CustomInput>
            </form>
        {:else}
            <h1>Sign Up</h1>
            <form>
                <CustomInput type="text" title="Nom du Propriétaire" required />
                <CustomInput type="text" title="Nom du Restaurant" required />
                <CustomInput type="email" title="Email" required />
                <CustomInput type="text" title="Adresse du siège social" required />
                <CustomInput type="number" title="Numéro de SIRET" required />
                <CustomInput type="tel" title="Numéro de téléphone du Restaurant" />
                <CustomInput type="url" title="Website url" />
                <CustomInput title="Nombre de place" type="number"/>
                <CustomInput type="select" title="Accès Handicapés" required 
                    option= {{
                        oui:'oui',
                        non:'non'
                    }}/>
                <CustomInput type="select" title="Terrasse" 
                    option= {{
                        oui:'oui',
                        non:'non'
                    }}/>
                <CustomInput type="select" title="Type de cuisine" 
                    option={{
                        traditionnel: 'Traditionnel',
                        gastronomique: 'Gastronomique',
                        bistrot: 'Bistrot',
                        brasserie: 'Brasserie',
                        cafe: 'Café',
                        rapide: 'Restauration rapide',
                        asiatique: 'Asiatique',
                        pizzeria: 'Pizzeria',
                        mediterraneen: 'Méditerranéen',
                        vegetarian: 'Végétarien',
                        vegan: 'Vegan',
                        glutenFree: 'Sans gluten',
                        bio: 'Bio',
                        foodTruck: 'Food Truck',
                        buffet: 'Buffet'
                      }}/>
                <CustomInput type="password" title="Password" required />
                <CustomInput type="password" title="Confirm Password" required />

                <CustomInput type="submit">Sign Up</CustomInput>
            </form>
        {/if}
        <!-- svelte-ignore a11y-missing-attribute -->
        <!-- svelte-ignore a11y-interactive-supports-focus -->
        <a role="CustomInput" on:click={toggle} on:keydown={toggle}>
            {#if signUp}Déja un compte?{:else}Créer un compte{/if}
        </a> 
    </div>
</main>

<style lang="scss">
    :global(main){
        width: 100%;
        height: 100vh;
        display: flex;
        justify-content: center;
        padding-top: var(--spacing);

        #container{
            background-color: var(--zomp);
            width: fit-content;
            height: fit-content;
            padding: var(--spacing);
            border-radius: var(--radius);

            h1{
                color: var(--black);
                margin-bottom: calc(var(--spacing) / 2);
            }

            form{
                display: flex;
                flex-direction: column;
                width: 20em;
                margin-bottom: var(--spacing);

                CustomInput[type="submit"]{
                    padding: calc(var(--spacing) / 4);
                    border-radius: var(--radius);
                    border: none;
                    background-color: var(--black);
                    color: var(--bone);
                    cursor: pointer;
                    font-size: 1.2em;
                    letter-spacing: .1em;

                    &:focus{
                        outline: none;
                    }
                }
            }

            a{
                color: var(--black);
                text-decoration: none;
                font-size: .8em;
                letter-spacing: .1em;
                cursor: pointer;

                &:focus{
                    outline: none;
                }
            
            }
        }
    }

    @media only screen and (max-width: 600px){
        :global(main){
            #container{
                width: 100vw;
                padding: var(--spacing) 0;
                border-radius: 0;

                h1{
                    padding: 0 calc(var(--spacing) / 2);
                }

                form{
                    padding: 0 calc(var(--spacing) / 2);
                }
            }
        }
    }
</style>