<template>
    <tr>
        <td><router-link :to="item.product.get_absolute_url">{{ item.product.name }}</router-link></td>
        <td>${{ item.product.price }}</td>
        <td>
            <div class="field has-addons">
                <p class="control">
                    <button class="button is-primary" @click="decrementQuantity(item)">-</button>
                </p>
                <p class="control">
                    <input class="input" type="number" v-model.number="item.quantity" min="0">
                </p>
                <p class="control">
                    <button class="button is-primary" @click="incrementQuantity(item)">+</button>
                </p>
            </div>
        </td>
        <td>${{ getItemTotal(item).toFixed(2) }}</td>
        <td><button class="delete" @click="removeFromCart(item)"></button></td>
    </tr>
</template>

<script>
export default {
    name: 'CartItem',
    props: {
        initialItem: Object
    },
    data() {
        return {
            item: this.initialItem
        }
    },
    methods: {
        getItemTotal(item) {
            return item.quantity * item.product.price
        },
        decrementQuantity(item) {
            if (item.quantity > 0) {
                item.quantity -= 1
                this.updateCart()
            }
        },
        incrementQuantity(item) {
            item.quantity += 1
            this.updateCart()
        },
        updateCart() {
            localStorage.setItem('cart', JSON.stringify(this.$store.state.cart))
        },
        removeFromCart(item) {
            this.$emit('removeFromCart', item)
            this.updateCart()
        },
    },
}
</script>
