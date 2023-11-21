<script lang="ts">
    import Template from "../../lib/template.svelte";
    import CustomInput from "../../lib/customInput.svelte";
    import { API_URL } from "../../main";
    import Modal from "../../lib/modal.svelte";

    let information = {
        firstName: "",
        lastName: "",
        email: "",
        companyName: "",
        socialAdresse: "",
        siret: ""
    }
    let error = "";
    let showModal = false;

    fetch(`${API_URL}/owner/profile`, {
            method: "GET",
            headers: {
                'Content-Type': 'application/json',
                'Authorization': 'Bearer ' + localStorage.getItem('token')
            }
        })
        .then((res) => res.json())
        .then((data) => {
            information = data.owner;
        })

    const handleUpdateAccount = () => {
        fetch(`${API_URL}/owner/profile`, {
                method: "PATCH",
                headers: {
                    'Content-Type': 'application/json',
                    'Authorization': 'Bearer ' + localStorage.getItem('token')
                },
                body: JSON.stringify(information)
            })
            .then((res) => res.json())
            .then((data) => {
                error = data.message;
            })
    }

    const handleDeleteAccount = () => {
        fetch(`${API_URL}/owner/profile`, {
                method: "DELETE",
                headers: {
                    'Content-Type': 'application/json',
                    'Authorization': 'Bearer ' + localStorage.getItem('token')
                }
            })
            .then((res) => {
                if (res.status === 200) {
                    localStorage.removeItem('token');
                    window.location.href = "/";
                }
                return res.json();
            })
            .then((data) => {
                error = data.message;
            })
    }
</script>

<Template>
    <h1>Edit Profil</h1>
    <div id="content">
        <div id="containerInput">
            <CustomInput title="Nom" type="text" bind:value={information["lastName"]} required/>
            <CustomInput title="Prénom" type="text" bind:value={information["firstName"]} required/>
            <CustomInput title="Email personnel" bind:value={information["email"]} type="email" required/>
        </div>
        <div id="containerInput">
            <CustomInput title="Adresse du siège social" type="text" bind:value={information["socialAdresse"]} required/>
            <CustomInput title="Numéro de siret" type="text" bind:value={information["siret"]} required disabled/> <!-- il es disable, il ne doit pas être envoyé quand il modifie quelquechose -->
            <CustomInput title="Nom de l'entreprise" type="text" bind:value={information["companyName"]} required/>
        </div>
        
    </div>
    <p class="error">{error}</p>
    <div id="submit">
        <button on:click={handleUpdateAccount} class="material-symbols-rounded save">save</button>
        <button on:click={() => showModal = true} class="material-symbols-rounded save">delete</button>
    </div>

    <Modal bind:showModal validate={handleDeleteAccount}>
        <h2 slot="header">Voulez-vous vraiment supprimer votre compte ?</h2>
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
    #content{
        display: flex;
        position: relative;
        width: 100%;
        flex-direction: row;

        #containerInput{
            display: flex;
            flex-direction: column;
            width: 50%;
            padding: var(--spacing);
        }
    }
    #submit{
        width: 100%;
        display: flex;
        justify-content: center;
        align-items: center;
        margin: var(--spacing) 0;
        gap: 1em;

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