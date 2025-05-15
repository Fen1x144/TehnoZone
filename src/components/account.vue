<script setup>
import { reactive, ref, onMounted } from "vue";
import useUsers from "../composables/useUsers"
import { RouterLink } from 'vue-router';
import { useRouter } from "vue-router";
import useOrders from "../composables/useOrders";


const authorizationpolzovatel = useUsers().findUser(useUsers().authorizationpolzovatel.value)
const idauthorizationpolzovatel = useUsers().authorizationpolzovatel;
const orders = useOrders().orders

const router = useRouter();

const bulls = reactive({
    bull1: true,
    bull2: false,
})

if (authorizationpolzovatel) {
    bulls.bull1 = false
    bulls.bull2 = true
}
else {
    bulls.bull1 = true
    bulls.bull2 = false
}

function exit() {
    useUsers().authorizationpolzovatel.value = NaN
    router.push('authorization')
}




function udluser() {
    if (authorizationpolzovatel.id == 0) {
        mainbull.mod1 = true
    }
    else {
        useUsers().deleteUser(useUsers().authorizationpolzovatel.value)
        router.push({ name: 'authorization' })
    }
}

const mainbull = reactive({
    mod1: false,
})

function accept() {
    mainbull.mod1 = false
}

const today = ref(new Date());
const orderNumber = ref('');



// Авторизация

const users = useUsers().users

const znachpolauth = reactive({
    nameauth: null,
    passwordauth: null,
    emailauth: null,
})

const errorsauth = reactive({
    ernameauth: '',
    erpasswordauth: '',
    ertemailauth: '',
    ernotpolzovatel: '',
})

function avtoriz() {

    if (znachpolauth.nameauth == null) {
        errorsauth.ernameauth = "Ошибка. Введено пустое поле"
    }
    else {
        errorsauth.ernameauth = ''
    }

    if (znachpolauth.passwordauth == null) {
        errorsauth.erpasswordauth = "Ошибка. Введено пустое поле"
    }
    else {
        errorsauth.erpasswordauth = ''
    }

    if (znachpolauth.emailauth == null) {
        errorsauth.ernotpolzovatel = "Ошибка. Введено пустое поле"
    }
    else {
        errorsauth.ernotpolzovatel = ''
    }


    if ((znachpolauth.nameauth != null) && (znachpolauth.passwordauth != null) && (znachpolauth.emailauth != null)) {
        // Ищем пользователя
        const foundUser = users.value.find(user =>
            znachpolauth.nameauth == user.name &&
            znachpolauth.passwordauth == user.password && znachpolauth.emailauth == user.email
        );

        if (foundUser) {
            useUsers().setAuthorizedUser(foundUser)
            router.push('/account')
            znachpolauth.nameauth = null
            znachpolauth.passwordauth = null
            znachpolauth.emailauth = null
        } else {
            errorsauth.ernotpolzovatel = "Введенный пользователь не зарегистрирован"
        }
    }

}

</script>
<template>
    <!-- Всплывающее окно -->
    <div class="window" v-if="mainbull.mod1">
        <br>
        <div class="window-main">
            <h2>Нельзя удалить Админа</h2>
            <br><br><br>
            <button class="buttonAccept" @click="accept()">Понял</button>
            <div class="window-back"></div>
        </div>
    </div>
    <!-- Авторизация -->
    <div v-if="bulls.bull1">
        <form id="auth-block">
            <h2 class="size">Авторизация пользователя</h2>
            <label for="inp3">Имя:</label><input type="text" id="inp3" placeholder="Введите имя"
                v-model="znachpolauth.nameauth">
            <p class="osh">{{ errorsauth.ernameauth }}</p>
            <label for="inp4">Пароль:</label><input type="text" id="inp4" placeholder="Введите пароль"
                v-model="znachpolauth.passwordauth">
            <p class="osh">{{ errorsauth.erpasswordauth }}</p><br>
            <label for="inp5">Почта:</label><input type="text" id="inp5" placeholder="Введите почту"
                v-model="znachpolauth.emailauth">
            <p class="osh">{{ errorsauth.ertemailauth }}</p>
            <p class="osh">{{ errorsauth.ernotpolzovatel }}</p>
            <button id="inp-auth" @click.prevent="avtoriz()">Авторизоваться</button>
            <p>Вы не зарегистрированы? <RouterLink :to="{ name: 'regestration' }" title="Личный кабинет">Зарегистрируйтесь</RouterLink>
            </p>
        </form>
    </div>
    <!-- Блок аккаунта -->
    <div v-if="bulls.bull2">
        <div class="acc-box">
            <div class="boxflex">
                <img src="../assets//avatar.jpg" alt="sigma" class="profilePic">
                <div class="flexrazdel">
                    <div class="data">
                        <h2>{{ authorizationpolzovatel.name }}</h2>
                        <p>Почта: {{ authorizationpolzovatel.email }}</p>
                    </div>
                </div>
                <div class="btns-account">
                    <button class="btn" @click="udluser()">Удалить</button>
                    <button class="btn" @click="exit()">Выйти</button>
                </div>
            </div>
        </div>
        <!-- Блок заказов -->
        <div class="order-box">
            <h2>Ваши заказы</h2>
            <hr>
            <div v-for="order in orders" :key="order.id" class="ordersbox">
                <div class="orders" v-if="order.authorizationpolzovatelId === idauthorizationpolzovatel">
                    <img class="icon-order" src="../assets/imgorder.png">
                    <div>
                        <h2>Заказ от {{ today.toLocaleDateString() }}</h2>
                        <p>Ваш номер заказа: {{ order.numer }}</p>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>
<style scoped>

/* Блок аккаунта */

.profilePic {
    width: 300px;
    height: 300px;
}

.boxflex {
    display: flex;
    gap: 100px;
}

.flexrazdel {
    display: flex;
    gap: 40px;
}

.btns-account {
    margin-top: 20px;
    margin-left: 200px;
    display: flex;
    gap: 20px;
    
}

.acc-box {
    width: 1270px;
    height: 300px;
    padding: 30px;
    border-radius: 10px;
    margin: 20px auto;
    border: 2px solid #dcdcdc;
    background-color: white;
}

.btn {
    width: 100px;
    height: 30px;
    border-radius: 10px;
    border: 1px solid #dcdcdc;
    background-color: #f0f0f0;
    color: black;
    font-weight: bold;
    transition: transform 0.3s ease;
}

.btn:hover {
    background-color: #d4d4d4;
}

.data {
    width: 300px;
}



.udluser {
    border-radius: 10px;
    border: 1px solid #dcdcdc;
    background-color: #f0f0f0;
    color: black;
    font-weight: bold;
    width: 150px;
    transition: transform 0.3s ease;
    height: 30px;
}

.udluser:hover {
    background-color: #E30613;
}

/* Блок заказов */

.ordersbox {
    margin-top: 40px;
    margin-bottom: 40px;
    display: flex;
    justify-content: center;
}

.order-box h2{
    text-align: center;
}

.orders {
    width: 1000px;
    height: 100px;
    border: 1px solid black;
    border-radius: 10px;
    display: flex;
    gap: 40px;
    padding: 20px;
    background-color: white;
}

.icon-order{
    width: 100px;
    height: 100px;
}

/* Авторизация */

input {
    margin-left: 20px;
}

.size {
    font-size: 24px;
}

form {
    width: 30%;
    padding: 30px;
    margin: 20px auto;
    border: 2px solid #dcdcdc;
    border-radius: 10px;
    overflow: hidden;
    position: relative;
    background-color: white;
}

form h2, p{
    text-align: center;
}

#inp3, #inp4, #inp5 {
    height: 30px;
    width: 250px;
    border: 2px solid #dcdcdc;
    border-radius: 5px;
}

#inp4 {
    margin-top: 20px;
}

#inp-auth {
    margin-top: 20px;
    background-color: #f0f0f0;
    width: 150px;
    height: 40px;
    border: 1px solid #dcdcdc;
    border-radius: 10px;
    transition: transform 0.3s ease;
    cursor: pointer;
    padding: 10px 20px;
}

#inp-auth:hover {
    background-color: #d4d4d4;
}

.osh {
    color: #E30613;
}

/* Всплываюшее окно */

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

.window h2{
    text-align: center;
    position: relative;
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

.buttonAccept {
    position: relative;
    width: 100px;
    height: 40px;
    border-radius: 10px;
    z-index: 1002;
    background-color: #f0f0f0;
    border: 1px solid #dcdcdc;
    margin-left: 200px;
    margin-top: 40px;
}

.buttonAccept:hover {
    background-color: #d4d4d4;
}
</style>