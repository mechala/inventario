<template id="product-list">
	<div class="container"> 
		<div class = "row">
			<div class="col-lg-8 col-sm-8 col-xs-5">
				<p class = "nuevoProducto">NUEVO PRODUCTO</p>
				<hr class="lineTitle"> 
			</div>
		</div>
		<form v-on:submit.prevent="createProduct">
			<div class="form-group">
				<div class="row">
					<div class="col-lg-4 col-sm-4 col-xs-3">
						<input v-model="products.name" class="form-control" id="add-name"  placeholder="Nombre" required/>
					</div>
					<div class="col-lg-2 col-sm-2 col-xs-2">
						<input v-model="products.price" class="form-control" id="add-price" placeholder="Precio" />
					</div>
					<div class="col-lg-2 col-sm-2 col-xs-2">
						<input v-model="products.quantity" class="form-control" id="add-price" placeholder="Cantidad"/>
					</div>
				</div>	
			</div>
			<div class="form-group">
					<div class="row">
						<div class="col-lg-6 col-sm-6 col-xs-4">
							<textarea v-model="products.description" class="form-control" id="add-description" rows="4" placeholder="Descripcion" />
						</div>
						<div class="col-lg-2 col-sm-2 col-xs-1">
							<button type="submit" class="btn btn-primary addproducts">Agregar</button>
						</div>
					</div>
			</div>  							 	 	 
		</form>
		<div class="row">
			<div class="form-group col-lg-8 col-sm-8 col-xs-5">
				<p class = "nuevoProducto">LISTADO DE PRODUCTOS</p>
				<hr class="lineTitle"> 
					<input v-model="search" class="form-control no-border glyphicon glyphicon-search" id="search-element"  placeholder="Filtrar" required/>   
					<table class="table table-striped">
					<thead>
					<tr>
					<th>Nombre</th>
					<th>Descripcion</th>
					<th>Precio</th>
					<th>Cant.</th>
					<th>Borrar</th>
					</tr>
				</thead>
				<tbody>
					<tr v-for="product in filteredProducts">
						<td class="nameProduct">
						<a  href="#" v-on:click="viewdetails(product.id)">{{ product.name }}</a>
						</td>
						<td>{{ product.description }}</td>
						<td>{{ product.price }}
						<span class="glyphicon glyphicon-euro" aria-hidden="true"/>
						<td>{{ product.quantity }}
						<span  aria-hidden="true">
						</span>
						</td>
						<td>
						<a class="btn btn-danger btn-xs" href="#" v-on:click="deleteproduct(product.id)">X</a>
						</td>
					</tr>
				</tbody>
			</table>
			</div>
			
			<div class="form-group col-lg-3 col-sm-3 col-xs-3">
				<p class = "detalleProducto">DETALLE</p>
				<hr class="lineTitle"> 

			<div class="row detailPrice">
			<div class="form-group col-lg-6 col-sm-6 col-xs-6">
				<p class = "txtdetalleProduct">{{producto.name}}</p>
				<hr class="lineproduct">
				<p class="price">${{producto.price}}</p>
				<span>{{producto.description}}</span>	
			</div>
			
			<div class="form-group col-lg-1 col-sm-1 col-xs-1">
				<div class="vertical-line" style="height: 200px;"></div>
			</div>
			<div class="form-group col-lg-4 col-sm-4 col-xs-4">
			<div class="circle-text">{{producto.quantity}}</div>
			<div class="in-creasedquantity">
			<a  class='qtyminus' value='-'  href="#" v-on:click="decrease(producto.id)">-</a>
    	<a class='qtyplus' value='+'   href="#" v-on:click="increased(producto.id)"  >+</a>
			</div>
			</div>	
			</div>
			</div>
		</div>	
	</div>
</template>
<script>
export default {
  name: 'HelloWorld',
  data () {
    return {
   producto : {id: 0, name: '', description: '', price: 0, quantity: 0},
	 search: '',
     products : [
  {id: 1, name: 'Pan de arequipe', description: 'Pan de harina relleno de arequipe cubierto de azucar', price: 2500, quantity: 100},
  {id: 2, name: 'Panochas', description: 'Panochas rellenas de bocadillo', price: 200, quantity: 150},
 
]
    }
  }, 
  methods: {
        deleteproduct(product_id){
			let i = this.products.map(item => item.id).indexOf(product_id)
			this.products.splice(i, 1)
			localStorage.setItem("products", JSON.stringify(this.products))
        },
        createProduct(e)
          {
            this.products.push({
            id: this.products.length +1,
            name: this.products.name,
            description: this.products.description,
            price: this.products.price,
            quantity: this.products.quantity
          })
					localStorage.setItem("products", JSON.stringify(this.products))
          this.products.name = ""
          this.products.description = ""
          this.products.price = ""
          this.products.quantity = ""
          e.preventDefault();
          },
          viewdetails(product_id)
          {
            this.producto.name = this.products[product_id-1].name
						this.producto.description = this.products[product_id-1].description
						this.producto.id = this.products[product_id-1].id
						this.producto.price = this.products[product_id-1].price
						this.producto.quantity = this.products[product_id-1].quantity
          },
					increased(product_id){
						this.products[product_id-1].quantity++
						this.producto.quantity++

					},

					decrease(product_id){
						this.products[product_id-1].quantity--
						this.producto.quantity--
					}
    },
  computed: {
    filteredProducts() {
		if (localStorage["products"]) {
			this.products = JSON.parse(localStorage.getItem("products"))
			console.log(JSON.parse(localStorage.getItem("products")))
}			
      return this.products.filter(item => {
         return item.name.toLowerCase().indexOf(this.search.toLowerCase()) > -1
      })
    }
  }
}
		</script>

		<style>
		</style>