<script>
    import CustomInput from "../../lib/customInput.svelte";
    import SHA256 from 'crypto-js/sha256';
    import { API_URL } from "../../main";

    let signIn = true;

    let email = "";
    let password = "";
    let confirmPassword = "";
    let name = "";
    let surname = "";
    let companyName = "";
    let address = "";
    let siret = "";

    let error = "";

    let toggle = () => {
        window.scrollTo(0, 0);
        signIn = !signIn;
    }

    const areStringValide = (...str) => {
        return str.every(str => str && str.trim() !== "");
    }

    const handleLoginSubmit = (e) => {
        e.preventDefault();
        if(!areStringValide(email, password)){
            error = "Please fill all the fields";
            return;
        }
        const apiRequest = API_URL + new URL(e.target.action).pathname;

        let passwordHash = SHA256(password).toString();

        console.log(passwordHash)
        
        const data = {
            email: email,
            password: passwordHash
        }

        fetch(apiRequest, {
                method: 'POST',
                headers: {
                    'Content-Type': 'application/json'
                },
                body: JSON.stringify(data)
            })
            .then((res) => res.json())
            .then((data) => {
                if (data.token) {
                    error = "";
                    localStorage.setItem("token", data.token);
                    window.location.href = "/home";
                } else {
                    error = data.message;
                }
            })
    }

    const handleRegisterSubmit = (e) => {
        e.preventDefault();
        if(!areStringValide(email, password, confirmPassword, name, surname, companyName, address, siret)){
            error = "Please fill all the fields";
            return;
        }
        if(!email.match(/^[a-zA-Z0-9_.+-]+@[a-zA-Z0-9-]+\.[a-zA-Z0-9-.]+$/)){
            error = "Invalid email address";
            return;
        }
        if(password != confirmPassword){
            error = "Passwords don't match";
            return;
        }
        let passwordHash = SHA256(password).toString();
        const data = {
            firstName: name,
            lastName: surname,
            email: email,
            companyName: companyName,
            password: passwordHash,
            siret: siret,
            socialAdresse: address,
        }

        const apiRequest = API_URL + new URL(e.target.action).pathname;

        fetch(apiRequest, {
                method: 'POST',
                headers: {
                    'Content-Type': 'application/json'
                },
                body: JSON.stringify(data)
            })
            .then((res) => res.json())
            .then((data) => {
                if (data.token) {
                    error = "";
                    localStorage.setItem("token", data.token);
                    window.location.href = "/";
                } else {
                    error = data.message;
                }
            })
    
    }

</script>

<div class="mainContainer">
    <!-- svelte-ignore a11y-no-static-element-interactions -->
    <div id="container">
        {#if signIn}
            <h1>Sign In</h1>
            <form action="/owner/login" on:submit|preventDefault={handleLoginSubmit}>
                <CustomInput type="email" title="Email" required bind:value={email} />
                <CustomInput type="password" title="Password" required bind:value={password} />
                <p class="error">{error}</p>
                <CustomInput type="submit" title="Sign In"/>
            </form>
        {:else}
            <h1>Sign Up</h1>
            <form action="/owner/register" on:submit|preventDefault={handleRegisterSubmit}>
                <CustomInput type="text" title="Nom" required bind:value={name}/>
                <CustomInput type="text" title="Prénom" required bind:value={surname} />
                <CustomInput type="email" title="Email" required bind:value={email} />
                <CustomInput type="text" title="Nom de l'entreprise" required bind:value={companyName} />
                <CustomInput type="text" title="Adresse du siège social" required bind:value={address} />
                <CustomInput type="number" title="Numéro de SIRET" required bind:value={siret} />
                <CustomInput type="password" title="Password" required bind:value={password} />
                <CustomInput type="password" title="Confirm Password" required bind:value={confirmPassword} />
                <p class="error">{error}</p>
                <CustomInput type="submit" title="Sign Up"/>
            </form>
        {/if}
        <!-- svelte-ignore a11y-missing-attribute -->
        <!-- svelte-ignore a11y-interactive-supports-focus -->
        <!-- svelte-ignore a11y-unknown-role -->
        <a role="CustomInput" on:click={toggle} on:keydown={toggle}>
            {#if !signIn}Déja un compte?{:else}Créer un compte{/if}
        </a> 
    </div>
</div>

<style lang="scss">
    .mainContainer{
        width: 100%;
        height: 100%;
        display: flex;
        justify-content: center;
    }

    .error{
        color: var(--error);
        text-align: center;
    }
    :global(main){
        width: 100%;
        height: 100vh;
        
        #container{
            background-color: var(--zomp);
            width: fit-content;
            height: fit-content;
            padding: var(--spacing);
            border-radius: var(--radius);
            margin: auto;
            

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