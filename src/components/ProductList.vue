<template>
	<!-- Women Banner Section Begin -->
	<section class="women-banner spad">
		<div class="container-fluid">
			<div class="row">
				<div class="col-lg-12 mt-5" v-if="products.length > 0">
					<carousel
						:items="3"
						class="product-slider"
						:nav="false"
						:autoplay="true"
						:dots="false"
					>
						<div
							class="product-item"
							v-for="itemProduct in products"
							:key="itemProduct.id"
						>
							<div class="pi-pic">
								<img v-bind:src="itemProduct.galleries[0].photo" alt="" />
								<ul>
									<li
										@click="
											saveCart(
												itemProduct.id,
												itemProduct.name,
												itemProduct.price,
												itemProduct.galleries[0].photo
											)
										"
										class="w-icon active"
									>
										<a href=""><i class="icon_bag_alt"></i></a>
									</li>
									<li class="quick-view">
										<router-link v-bind:to="'/product/' + itemProduct.id"
											>+ Quick View
										</router-link>
									</li>
								</ul>
							</div>
							<div class="pi-text">
								<div class="catagory-name">{{ itemProduct.type }}</div>
								<router-link to="/product">
									<h5>{{ itemProduct.name }}</h5>
								</router-link>
								<div class="product-price">Rp. {{ itemProduct.price }}</div>
							</div>
						</div>
					</carousel>
				</div>
				<div class="col-lg-12" v-else>
					<p>Produk terbaru belum tersedia untuk saat ini</p>
				</div>
			</div>
		</div>
	</section>
	<!-- Women Banner Section End -->
</template>

<script>
import carousel from "vue-owl-carousel";
import axios from "axios";

export default {
	name: "ProductList",
	components: {
		carousel,
	},
	data() {
		return {
			products: [],
			cartUser: [],
		};
	},
	methods: {
		saveCart(idProduct, nameProduct, priceProduct, photoProduct) {
			var productStored = {
				id: idProduct,
				name: nameProduct,
				price: priceProduct,
				photo: photoProduct,
			};
			this.cartUser.push(productStored);
			const parsed = JSON.stringify(this.cartUser);
			localStorage.setItem("cartUser", parsed);
		},
	},
	mounted() {
		if (localStorage.getItem("cartUser")) {
			try {
				this.cartUser = JSON.parse(localStorage.getItem("cartUser"));
			} catch (e) {
				localStorage.removeItem(e);
			}
		}
		axios
			.get("http://127.0.0.1:8000/api/products", {
				params: {
					id: this.$route.params.id,
					name: this.$route.params.name,
					price: this.$route.params.price,
					photo: this.$route.params.photo,
				},
			})
			.then(res => (this.products = res.data.data.data))
			.catch(err => console.log(err));
	},
};
</script>

<style scoped>
.product-item {
	margin-right: 26px;
}
</style>
