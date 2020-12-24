<script>
    import { createEventDispatcher } from "svelte";
    import Title from "./Title.svelte";

    export let name;
    export let category;
    export let amount;
    export let id;
    export let isEditing = false;

    export let hideForm;
    export let showForm;

    $: isEmpty = !name || !amount || !category;
    const dispatch = createEventDispatcher();

    const handleSubmit = () => {
        showForm();
        if (isEditing) {
            dispatch("add-item", {
                id: id,
                name: name,
                amount: amount,
                category: category,
            });
        } else {
            dispatch("add-item", {
                id: Math.random(),
                name: name,
                amount: amount,
                category: category,
            });
        }

        id = null;
        name = "";
        amount = null;
        category = "";
        hideForm();
    };
</script>

<section class="form">
    <Title title="add item" />
    <form class="product-form">
        <div class="form-control">
            <label for="name">name</label>
            <input type="text" id="name" bind:value={name} />
        </div>
        <div class="form-control">
            <label for="name">amount</label>
            <input type="number" id="amount" bind:value={amount} />
        </div>
        <div class="form-control">
            <label for="category">category</label>
            <input type="text" id="category" bind:value={category} />
        </div>
        {#if isEmpty}
            <p class="form-empty">please fill out all form fields</p>
        {/if}
        <button
            type="submit"
            class="btn btn-block"
            class:disabled={isEmpty}
            disabled={isEmpty}
            on:click|preventDefault={handleSubmit}>{#if isEditing}
                edit item
            {:else}add item{/if}</button>
        <button
            type="button"
            class="close-btn"
            on:click|preventDefault={hideForm}>
            <i class="fas fa-times" />
            close
        </button>
    </form>
</section>
