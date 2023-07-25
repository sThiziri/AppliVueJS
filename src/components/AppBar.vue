<template>
    <div>
        <v-toolbar style="background-color: black;">


            <router-link to="/aporpos">
                <div>
                    <img src="@/assets/logo.png" class="float-left" alt="Logo">
                </div>
            </router-link>

            <v-spacer></v-spacer>
            <v-dialog transition="dialog-top-transition" width="auto">
                <template v-slot:activator="{ props }">
                    <v-btn icon v-bind="props">
                        <v-badge :content="shoppingCart?.numberOfProducts">
                            <v-icon icon="fa:fas fa-shopping-cart" size="x-large" color="warning"></v-icon>
                        </v-badge>
                    </v-btn>
                </template>
                <template v-slot:default="{ isActive }">
                    <v-card>
                        <v-toolbar color="primary" title="Panier"></v-toolbar>
                        <v-card-text>
                            <v-table height="300px">
                                <thead>
                                    <tr>
                                        <th class="text-left">
                                            Produits
                                        </th>
                                        <th class="text-left">
                                            Prix
                                        </th>
                                        <th class="text-left">
                                            Quantité
                                        </th>
                                    </tr>
                                </thead>
                                <tbody>
                                    <tr v-for="product in shoppingCart.productsList" :key="product.id">
                                        <td>{{ product.name }}</td>
                                        <td>{{ product.price * product.quantity }}</td>
                                        <td>
                                            <v-btn v-show="product.quantity > 0" icon="mdi-plus" size="x-small"
                                                color="secondary" variant="flat"
                                                @click="handleDecrementQuantity(product.Id, product.type)">-</v-btn>
                                            {{ product.quantity }}
                                            <v-btn icon="mdi-plus" size="x-small" color="secondary" ref="productRef"
                                                variant="flat"
                                                @click="handleIncrementQuantity(product.Id, product.type)">+</v-btn>
                                        </td>
                                    </tr>
                                </tbody>
                            </v-table>

                            <div class="text ">Nombre total de produit: <v-btn variant="flat">{{
                                shoppingCart.numberOfProducts }}</v-btn> </div>
                            <div class="text ">Total à payer: <v-btn variant="flat">{{ shoppingCart.totalCost }} €</v-btn>
                            </div>
                        </v-card-text>
                        <v-card-actions class="justify-end">
                            <v-btn variant="text" @click="isActive.value = false">Fermer</v-btn>
                        </v-card-actions>
                    </v-card>
                </template>
            </v-dialog>

        </v-toolbar>

    </div>
</template>

<script lang="ts" setup>
const { shoppingCart } = defineProps(['shoppingCart']);

const emit = defineEmits();

const handleIncrementQuantity = (productId: any, type: String) => {
    console.log("..." + productId);
    const eventData = { productId: productId, productType: type };
    emit('incrementQuantityCart', eventData);
}

const handleDecrementQuantity = (productId: any, type: String) => {
    console.log("..." + productId);
    const eventData = { productId: productId, productType: type };
    emit('decrementQuantityCart', eventData);
} 
</script>


<script lang="ts">
export default {
    emits: ['incrementQuantityCart', 'decrementQuantityCart'], // Define the custom event that the child component can emit
};
</script>