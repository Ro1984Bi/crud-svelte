<script>
import {v4} from 'uuid'
import Noty from 'noty'
import 'noty/lib/noty.css'
import 'noty/lib/themes/sunset.css'
	let products = [
		{
			id: 1,
			name: 'hp celerom',
			description: 'hp laptop',
			category: 'Laptops'
		},
		{
			id: 2,
			name: 'Ecopower',
			description: 'headphones',
			category: 'Peripherials'
		}

	];

	let product = {
		id: '',
		name: '',
		description: '',
		category: '',
		imageURL: ''
	};

	let editStatus = false;

	const deleteProduct = (id) => {
		products = products.filter(product => product.id !== id)
	}

	const cleanProduct = () => {
		product = {
			id: '',
			name: '',
			description: '',
			category: '',
			imageURL: '',
		}
	}

	const addProduct = () => {
		const newProduct = {
			id: v4(),
			name: product.name,
			description: product.description,
			category: product.category,
			imageURL: product.imageURL,
		}
		products = products.concat(newProduct)
		cleanProduct();
		console.log(products)
	}

	const updateProduct = () => {
		let updatedProduct = {
			id: product.id,
			name: product.name,
			description: product.description,
			category: product.category,
			imageURL: product.imageURL,
		}

		const productIndex = products.findIndex(p => p.id === product.id)
		products[productIndex] = updatedProduct;
		cleanProduct();
		editStatus = false
		new Noty({
			theme: 'sunset',
			type: 'success',
			timeout: 3000,
			text: 'Product updated successfully'
		}).show()
		                  
	}
	              

	const onSubmitHandler = e => {
		if (!editStatus) {
		addProduct();
		} else {
			updateProduct();  	 
		}
	}

	const editProduct = pedit => {
		product = pedit;
		editStatus = true;
	}
</script>

<main>
	<div class="container p-4">
		<div class="row">
			<div class="col-md-6">
				{#each products as product }
					<div class="card mt-2">
						<div class="row">
							<div class="col-md-4">
								{#if !product.imageURL}
								<img src="img/noProduct.png" alt="" class="img-fluid p-2"/>
								{:else}
								<img src="{product.imageURL}" alt="" class="img-fluid p-2"/>
								{/if}
							</div>
							<div class="col-md-8">
								<div class="card-body">
								<h5>
									<strong>{product.name}</strong>
									<span>
										<small>
											{product.category}
										</small>
									</span>	
								</h5>
								</div>
								<p class="card-text">{product.description}</p>
								<button class="btn btn-danger" on:click={deleteProduct(product.id)}>
									Delete
								</button>
							<button class="btn btn-secondary"  on:click={editProduct(product)}>
								Edit
							</button>	
							</div>
						</div>
					</div>
				{/each}
			</div>
			<div class="col-md-6">
				<div class="card">
					<div class="card-body">
						<form on:submit|preventDefault={onSubmitHandler}>
							<div class="form-group">
							<input bind:value={product.name} type="text" placeholder="Product name"
							 id="product-name" class="form-control"/>
							</div>
							<div class="form-group">
							<textarea bind:value={product.description}
							id="product-description" 
							rows="3" 
							placeholder="Product description" 
							class="form-control"/>
							</div>
							<div class="form-group">
							<input 
							bind:value={product.imageURL}
							type="url" 
							id="product-img"
							placeholder="https://rotechs.netlify.app"
							class="form-control"/>
							</div>
							<div class="form-group">
							<select id="category"
							bind:value={product.category} class="form-control">
								<option value="laptops">Laptops</option>
								<option value="peripherials">Peripherials</option>
								<option value="servers">Servers</option>
							</select>
							</div>
					
							<button class="btn btn-secondary">
								{#if !editStatus }Save{:else}Update{/if}
							</button>
						</form>
					</div>
				</div>
			</div>
		</div>
	</div>



</main>

<style>
</style>