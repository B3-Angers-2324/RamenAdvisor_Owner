<script>
    export let type = "";
    export let title = "".toLowerCase();
    export let required = false;
    export let option = {};
</script>

<buttonCustom>
    {#if type!="select"}
        <label for="{title}">{required ? `${title}*` : title}</label>
        <input type="{type}" name="{title}" id="{title}" placeholder="{title}" {required}>
    {:else}
        <label for="{title}">{title}</label>
        <select name="{title}" id="{title}">
            <option value="" disabled selected>---</option>
            {#each Object.keys(option) as key}
                <option value="{key}">{option[key]}</option>
            {/each}
        </select>
    {/if}
</buttonCustom>

<style lang="scss">
    buttonCustom{
        display: flex;
        flex-direction: column;
        width: 100%;
        
        label{
            color: var(--black);
            margin-bottom: .5em;
        }
        
        input, select{
            margin-bottom: calc(var(--spacing) / 2);
            padding: calc(var(--spacing) / 4);
            border-radius: var(--radius);
            border: none;
            background-color: var(--brunswick-green);
            color: var(--bone);
    
            &:focus{
                outline: none;
            }
        }

        /* Chrome, Safari, Edge, Opera */
        input::-webkit-outer-spin-button,
        input::-webkit-inner-spin-button {
        -webkit-appearance: none;
        margin: 0;
        }

        /* Firefox */
        input[type=number] {
        -moz-appearance: textfield;
        }
    }

</style>