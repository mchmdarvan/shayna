<template>
	<div class="home">
		<HeaderShayna />
		<HeroShayna />
		<ProductList />
		<InstaShayna />
		<PartnerShayna />
		<FooterShayna />
	</div>
</template>

<script>
// @ is an alias to /src
import HeaderShayna from "../components/HeaderShayna.vue";
import HeroShayna from "../components/HeroShayna.vue";
import ProductList from "../components/ProductList.vue";
import InstaShayna from "../components/InstaShayna.vue";
import PartnerShayna from "../components/PartnerShayna.vue";
import FooterShayna from "../components/FooterShayna.vue";

export default {
	name: "home",
	components: {
		HeaderShayna,
		HeroShayna,
		ProductList,
		InstaShayna,
		PartnerShayna,
		FooterShayna,
	},
};
</script>
