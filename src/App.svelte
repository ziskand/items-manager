<script>
	import { onMount, afterUpdate } from "svelte";

	import ProductsList from "./ProductsList.svelte";
	import Navbar from "./Navbar.svelte";
	import Total from "./Total.svelte";
	import ProductForm from "./ProductForm.svelte";
	import Modal from "./Modal.svelte";

	let products = [];

	let name = "";
	let amount = null;
	let category = null;
	let id = null;

	let isFormOpen = false;

	$: isEditing = id ? true : false;

	$: total = products.reduce((acc, curr) => (acc += curr.amount), 0);

	const showForm = () => {
		isFormOpen = true;
	};

	const hideForm = () => {
		isFormOpen = false;
	};

	const removeItem = (e) => {
		products = products.filter((item) => item.id !== e.detail);
	};

	const addItem = (e) => {
		let exisitngItem = products.find((item) => item.id === e.detail.id);
		if (!exisitngItem) {
			products = [e.detail, ...products];
			console.log(products);
		} else {
			products = products.map((item) => {
				return item.id === e.detail.id ? { ...e.detail } : { ...item };
			});

			console.log(products);
		}
		name = "";
		amount = null;
		category = null;
		id = null;
	};

	const editSelectedItem = (e) => {
		id = e.detail.id;
		name = e.detail.name;
		amount = e.detail.amount;
		category = e.detail.category;
	};

	const removeAllItems = () => {
		products = [];
	};

	const setLocalStorage = () => {
		localStorage.setItem("products", JSON.stringify(products));
	};

	onMount(() => {
		let localStprageProducts = localStorage.getItem("products");

		products = localStprageProducts ? JSON.parse(localStprageProducts) : [];
	});

	afterUpdate(() => {
		setLocalStorage();
	});
</script>

<Navbar {showForm} />
<main class="content">
	{#if isFormOpen}
		<Modal>
			<ProductForm
				on:add-item={addItem}
				{name}
				{amount}
				{category}
				{isEditing}
				{id}
				{hideForm}
				{showForm} />
		</Modal>
	{/if}
	<Total title="total amount" {total} />
	<ProductsList
		{products}
		{showForm}
		on:remove-item={removeItem}
		on:edit-item={editSelectedItem} />

	{#if products.length !== 0}
		<button
			type="button"
			class="btn btn-primary btn-block"
			on:click|preventDefault={removeAllItems}>
			remove all items
		</button>
	{/if}
</main>
