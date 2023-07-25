<template>
    <v-card class="mx-auto" width="350" height="350">
        <v-img :src="details.imgLink" height="200px" cover></v-img>

        <v-card-title>
            {{ details?.name }}
        </v-card-title>

        <v-card-subtitle>
            {{ details?.description }}
        </v-card-subtitle>

        <v-card-actions>
            <v-btn color="orange-lighten-2" variant="text">
                {{ details?.price }} €
            </v-btn>

            <v-spacer></v-spacer>

            <div v-show="showAddBtn">
                <v-btn variant="flat" @click="handleAddBtnClick">
                    Ajouter
                </v-btn>
            </div>

            <div v-show="!showAddBtn">
                <v-btn v-show="details.quantity > 0" icon="mdi-plus" size="x-small" color="secondary" variant="flat" @click="handleDecrementQuantity">-</v-btn>
                <span>{{ details?.quantity }}</span>
                <v-btn icon="mdi-plus" size="x-small" color="secondary" variant="flat" @click="handleIncrementQuantity">+</v-btn>
            </div>

        </v-card-actions>

        <v-expand-transition>
            <div v-show="show">
                <v-divider></v-divider>

                <v-card-text>
                    {{ details?.Description }}
                </v-card-text>
            </div>
        </v-expand-transition>
    </v-card>
</template>

<script lang="ts" setup>

import { ref } from 'vue';
//import {  defineEmits } from 'vue';
const emit = defineEmits();

let show = true;
let showAddBtn = ref(true);

const { details } = defineProps(['details']);

const handleAddBtnClick = () => {
    showAddBtn.value = false;
    handleIncrementQuantity();
}

const handleIncrementQuantity = () => {
    const eventData = { productId: details.Id };
    emit('incrementQuantity', eventData);
}

const handleDecrementQuantity = () => {
    
    const eventData = { productId: details.Id };
    emit('decrementQuantity', eventData);

    if(details.quantity == 0){
        showAddBtn.value = true;
    }
}

</script>

<script lang="ts">
export default {
  emits: ['incrementQuantity', 'decrementQuantity'], // Define the custom event that the child component can emit
};
</script>