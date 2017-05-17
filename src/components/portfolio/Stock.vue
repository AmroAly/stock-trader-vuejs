<template>
	<div class="col-sm-6 col-md-4">
		<div class="panel panel-info">
			<div class="panel-heading">
				<h3 class="panel-title">
					{{ stock.name }}
					<small>(Price: {{ stock.price }} | Quantity {{ stock.quantity }})</small>
				</h3>
				<div class="panel-body">
					<div class="pull-left">
						<input type="number" class="form-control" placeholder="Quantity" v-model="quantity"
						:class="{danger: insuffecientQuatity}">
					</div>
					<div class="pull-right">
						<button class="btn btn-info" @click="sellStock" 
						:disabled="insuffecientQuatity || quantity <= 0 || Number.isInteger(quantity)">
							{{ insuffecientQuatity ? 'Not enough': 'Sell' }}
						</button>
					</div>
				</div>
			</div>
		</div>
	</div>
</template>

<script>
	import { mapActions } from 'vuex';
	export default {
		props: ["stock"],
		data() {
			return {
				quantity: 0
			}
		},
		computed: {
			insuffecientQuatity() {
				return this.quantity > this.stock.quantity;
			}
		},
		methods: {
			...mapActions({
				placeSellOrder: 'sellStock'
			}),
			sellStock() {
				const order = {
					stockId: this.stock.id,
					stockPrice: this.stock.price,
					quantity: this.quantity
				}
				this.placeSellOrder(order);
				this.quantity = 0;
			}
		}
	}
</script>

<style scoped>
	.danger {
		border: 1px solid red;
	}
</style>