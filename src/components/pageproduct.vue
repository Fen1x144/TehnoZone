<script setup>

import { RouterLink, useRoute, useRouter } from 'vue-router';
import useProducts from '../composables/useProducts';
import { computed, reactive, ref } from 'vue';
import useUsers from '../composables/useUsers';
import useBasket from '../composables/useBasket';

const basketproducts = useBasket().basketproducts
const route = useRoute()
const products = useProducts().products;
const authorizationpolzovatel = useUsers().authorizationpolzovatel
const localproduct = computed(() => useProducts().findProduct(Number(route.params.id)))

const statusproductbasket = ref()

function dobavincorsina(productId, authorizationpolzovatel) {
    statusproductbasket.value = "";
    if (authorizationpolzovatel >= 0) {
        basketproducts.value.forEach(basketproduct => {
            if ((basketproduct.productId == productId) && (basketproduct.authorizationpolzovatelId == authorizationpolzovatel)) {
                statusproductbasket.value = "Товар уже добавлен в корзину";
            }
        });
        if (!statusproductbasket.value) {
            useBasket().addProductbasket(productId, authorizationpolzovatel);
        }
    } else {
        alert("Нет авторизованного пользователя");
    }
}




</script>

<template>

    <section>
        <div class="card-product">
            <img :src=localproduct.imgproduct alt="" class="imgtovar">
            <div class="divider"></div>
            <div class="datatovar">
                <p class="title">{{ localproduct.title }}</p>
                <p>{{ localproduct.description }}</p>
                <p class="pricetop">Цена:{{ localproduct.price }} рублей</p>
                <p class="proverkadobav">{{ statusproductbasket }}</p> <!-- Отображение информации о добавлении товара в корзину -->
                <button @click="dobavincorsina(localproduct.id, authorizationpolzovatel)">Добавить в корзину</button> <!-- Добавление товара -->
            </div>
        </div>
    </section>
</template>

<style scoped>

/* Описание товара */

.proverkadobav {
    color: #E30613;
}

.title {
    font-size: 22px;
}

.card-product {
    display: flex;
    justify-content: center;
    width: 1000px;
    height: 400px;
    padding: 40px;
    gap: 120px;
    margin: 20px auto;
    background-color: white;
    border: 2px solid #dcdcdc;
    border-radius: 10px;
}

/* Кнопка */

button {
    width: 120px;
    height: 35px;
    background-color: #f0f0f0;
    border: 1px solid #dcdcdc;
    font-family: 600;
    border-radius: 5px;
}

button:hover {
    background-color: #d4d4d4;
}

/* Разделитель */

.divider{
    width: 1px;
    height: 300px;
    background-color: #d1d1d1;
}

/* Фото товара */

.imgtovar {
    width: 300px;
    height: 300px;
    margin-left: 25px;
}

</style>