<script>
    import { fly } from "svelte/transition";
    import { flip } from "svelte/animate";

    import Product from "./Product.svelte";
    import Title from "./Title.svelte";
    export let products = [];
    export let showForm;
</script>

<style>
    h2 {
        text-transform: capitalize;
    }
</style>

<section>
    <Title title="All items" />
    {#each products as product, index (product.id)}
        <div
            in:fly={{ x: 200, delay: index * 700 }}
            out:fly={{ x: -200 }}
            animate:flip>
            <Product {...product} {showForm} on:remove-item on:edit-item />
        </div>
    {:else}
        <h2>your watch list is empty</h2>
    {/each}
</section>
