<template>
	<div class="product">
		<HeaderShayna />
		<!-- Breadcrumb Section Begin -->
		<div class="breacrumb-section text-left">
			<div class="container">
				<div class="row">
					<div class="col-lg-12">
						<div class="breadcrumb-text product-more">
							<router-link to="/"><i class="fa fa-home"></i> Home</router-link>
							<span>Detail</span>
						</div>
					</div>
				</div>
			</div>
		</div>
		<!-- Breadcrumb Section Begin -->

		<!-- Product Shop Section Begin -->
		<section class="product-shop spad page-details">
			<div class="container">
				<div class="row">
					<div class="col-lg-12">
						<div class="row">
							<div class="col-lg-6">
								<div class="product-pic-zoom">
									<img class="product-big-img" :src="defaultPict" alt="" />
								</div>
								<div
									class="product-thumbs"
									v-if="productDetails.galleries.length > 0"
								>
									<carousel
										class="product-thumbs-track ps-slider"
										:nav="false"
										:dots="false"
										:autoplay="true"
									>
										<div
											v-for="slideshow in productDetails.galleries"
											:key="slideshow.id"
											class="pt"
											@click="changeImage(slideshow.photo)"
											:class="slideshow.photo == defaultPict ? 'active' : ''"
										>
											<img :src="slideshow.photo" alt="" />
										</div>
									</carousel>
								</div>
							</div>
							<div class="col-lg-6">
								<div class="product-details text-left">
									<div class="pd-title">
										<span>{{ productDetails.type }}</span>
										<h3>{{ productDetails.name }}</h3>
									</div>
									<div class="pd-desc">
										<p v-html="productDetails.description"></p>
										<h4>Rp.{{ productDetails.price }}</h4>
									</div>
									<div class="quantity">
										<router-link to="/cart">
											<a
												@click="
													saveCart(
														productDetails.id,
														productDetails.name,
														productDetails.price,
														productDetails.galleries[0].photo
													)
												"
												href=""
												class="primary-btn pd-cart"
												>Add To Cart</a
											>
										</router-link>
									</div>
								</div>
							</div>
						</div>
					</div>
				</div>
			</div>
		</section>
		<!-- Product Shop Section End -->

		<RelatedProduct />

		<FooterShayna />
	</div>
</template>

<script>
// @ is an alias to /src
import HeaderShayna from "../components/HeaderShayna.vue";
import RelatedProduct from "../components/RelatedProduct.vue";
import FooterShayna from "../components/FooterShayna.vue";

import axios from "axios";

import carousel from "vue-owl-carousel";

export default {
	name: "product",
	components: {
		HeaderShayna,
		RelatedProduct,
		FooterShayna,
		carousel,
	},
	data() {
		return {
			defaultPict: "",
			productDetails: [],
			cartUser: [],
		};
	},
	methods: {
		changeImage(urlImage) {
			this.defaultPict = urlImage;
		},
		setDataPicture(data) {
			// replace object productDetails dengan data dari API
			this.productDetails = data;
			//replace value gambar default dengan data dari API (galleries)
			this.defaultPict = data.galleries[0].photo;
		},
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
			.then(res => this.setDataPicture(res.data.data))
			// eslint-disable-next-line no-console
			.catch(err => console.log(err));
	},
};
</script>

<style scoped>
.product-thumbs .pt {
	margin-right: 10px;
	max-height: 10rem;
}
.product-pic-zoom img {
	max-width: 100%;
	max-height: 34rem;
}
</style>
