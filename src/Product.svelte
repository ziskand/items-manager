<script>
    import { slide } from "svelte/transition";
    import { createEventDispatcher } from "svelte";
    export let id;
    export let name = "";
    export let amount = 0;
    export let category = "";

    export let showForm;

    let isProductSelected = false;

    const toggleProducDetailsSelected = () => {
        isProductSelected = !isProductSelected;
    };

    const dispatch = createEventDispatcher();
</script>

<article class="single-product">
    <div class="product-info">
        <h2>
            {name}
            <button
                class="product-details-btn"
                on:click={toggleProducDetailsSelected}>
                <i class="fas fa-caret-down" /></button>
        </h2>
        {#if isProductSelected}
            <h4 transition:slide>units: {amount} category:{category}</h4>
        {/if}
    </div>
    <div class="product-buttons">
        <button
            class="product-btn edit-btn"
            on:click|preventDefault={() => {
                dispatch('edit-item', {
                    id: id,
                    name: name,
                    amount: amount,
                    category: category,
                });
                showForm();
            }}>
            <i class="fas fa-pen" />
        </button>
        <button
            class="product-btn delete-btn"
            on:click|preventDefault={() => dispatch('remove-item', id)}>
            <i class="fas fa-trash" />
        </button>
    </div>
</article>
