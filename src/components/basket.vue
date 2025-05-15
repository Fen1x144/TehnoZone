<script setup>
import useBasket from '../composables/useBasket';
import { computed, reactive, ref } from 'vue';
import useUsers from '../composables/useUsers';
import useProducts from '../composables/useProducts';
import useOrders from '../composables/useOrders';

const authorizationpolzovatel = useUsers().authorizationpolzovatel;
const basketproducts = useBasket().basketproducts;

const massiv = computed(() => {
    return basketproducts.value
        .filter(basketproduct => basketproduct.authorizationpolzovatelId === authorizationpolzovatel.value)
        .map(basketproduct => useProducts().findProduct(basketproduct.productId))
        .filter(product => product !== undefined);
});


const access = reactive({
    notAuth: computed(() => isNaN(authorizationpolzovatel.value)),
    auth: computed(() => authorizationpolzovatel.value >= 0),
    auth0: computed(() => massiv.value.length === 0),
    auth1: computed(() => massiv.value.length > 0),
});


function deleteProduct(productId) {
    useBasket().deleteProductbasket(productId);
}

const container = reactive({
    active: false,
})

function cancel() {
    container.active = false
}

function order() {
    container.active = true
}

const orderNumber = ref('');

function placeAnOrder() {
    if (!numcard.value) {
        error.value = alert('Поле не может быть пустым');
    } else if (!regularnumcard.value.test(numcard.value)) {
        error.value = alert('Неверный ввод. Номер карты должен состоять из 4 цифр.');
    } else {
        error.value = '';
        useOrders().addOrders(authorizationpolzovatel.value, orderNumber.value = 'SIGMO-' + new Date().getTime() + '-' + Math.floor(Math.random() * 1000))
        numcard.value = null;
        container.active = false;
        alert('Ваш заказ успешно оформлен!')
        deleteProduct(product.id)
    }
}

const regularnumcard = ref(/^\d{4}$/)
const numcard = ref(null)
const error = ref("")


</script>

<template>
    <!-- Всплывающие окно -->
    <div class="window" v-if="container.active">
        <div class="window-main">
            <div class="conInp">
                <div class="inp"><label for="numCard">Код карты:</label><input
                        title="Номер карты должен состоять из 4 цифр." v-model="numcard" type="text" id="numCard">
                </div>
                <p class="cardError">{{ error }}</p>
            </div>
            <div class="btnsflex">
                <button @click="cancel()">Отмена</button>
                <button @click="placeAnOrder()">Оформить</button>
            </div>
        </div>
        <div class="window-back"></div>
    </div>
    <div v-if="access.notAuth">
        <p class="note">Для доступа к корзине необходимо авторизоваться</p>
    </div>
    <!-- Корзина -->
    <div v-if="access.auth">
        <div v-if="access.auth0">
            <p class="basketEmpty">Корзина пуста</p>
        </div>
        <div class="con-basket" v-if="access.auth1">    
            <ul>
                <li v-for="product in massiv" :key="product.productId">
                    <img class="imgproduct" :src="product.imgproduct">
                    <div class="dataProduct">
                        <p>Название товара: {{ product.title }}</p>
                        <p>Цена товара: {{ product.price }} руб</p>
                    </div>
                    <button class="buttonBasket" @click="deleteProduct(product.id)">Удалить из корзины</button>
                </li>
            </ul>
            <div class="buttonUnderBasket">
                <button @click="order()">Оформить заказ</button>
            </div>
        </div>
        
    </div>
</template>

<style scoped>
/* Корзина */

.basketEmpty {
    text-align: center;
    margin-top: 350px;
    font-size: 40px;
    font-weight: 600;
}

.dataProduct {
    width: 650px;
}

.buttonBasket {
    height: 40px;
    width: 200px;
    margin-top: 40px;
    background-color: #f0f0f0;
    border: 1px solid #dcdcdc;
    border-radius: 10px;
    cursor: pointer;
}

.buttonUnderBasket{
    display: flex;
    justify-content: center;
}

.buttonUnderBasket button{
    height: 40px;
    width: 200px;
    background-color: white;
    border: 1px solid #dcdcdc;
    border-radius: 10px;
    cursor: pointer;
}

.buttonUnderBasket button:hover{
    background-color: #d4d4d4;
}

.buttonBasket:hover {
    background-color: #d4d4d4;
}

.imgproduct {
    width: 100px;
    height: 100px;
}

.note {
    text-align: center;
    margin-top: 350px;
    font-size: 40px;
    font-weight: 600;
}

ul {
    list-style-type: none;
    padding: 0;
}

li {
    margin: 20px auto;
    padding: 30px;
    background-color: #fff;
    border: 1px solid #ddd;
    border-radius: 15px;
    height: 120px;
    width: 1000px;
    display: flex;
    gap: 20px;
}

/* Всплывающие окно */

.window {
    top: 0;
    left: 0;
    width: 100vw;
    height: 100vh;
    position: fixed;
    display: flex;
    align-items: center;
    justify-content: center;
    z-index: 1001;
}

.window-main {
    background-color: white;
    border: 2px solid black;
    border-radius: 10px;
    height: 250px;
    width: 500px;
}

.window-back {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    z-index: 1000;
    background-color: white;
    opacity: 0.5;

}

.window button {
    position: relative;
    width: 100px;
    height: 40px;
    border-radius: 10px;
    z-index: 1002;
    background-color: #f0f0f0;
    border-radius: 10px;
    border: 1px solid #dcdcdc;
}

.window button:hover {
    background-color: #d4d4d4;
}

#numCard{
    margin-left: 20px;
    border: 1px solid #dcdcdc;
    border-radius: 5px;
}

.conInp {
    height: 60px;
}

.inp {
    position: relative;
    z-index: 1001;
    margin-left: 120px;
    margin-top: 40px;
}

.cardError {
    position: relative;
    z-index: 1001;
    color: #E30613;
    margin-left: 120px;
}

.btnsflex {
    display: flex;
    gap: 20px;
    margin-left: 130px;
    margin-top: 60px;
}
</style>
