<script setup>
import { ref, reactive } from 'vue';

const comment = ref("")
const email = ref("")
const mailError = ref("")
const commError = ref("")

// Функция отправки комментария
function send() {
    if (comment.value == "") {
        commError.value = alert('Пожалуйста, напишите комментарий.')
    }
    else {
        container.active = true
        comment.value = "";
        commError.value = ""
    }
}

// Функция подписки на рассылку 
function follow(){
    if (email.value == "") {
        mailError.value = alert("Пожалуйста, введите вашу почту.")
    } else {
        container.active1 = true;
        email.value = "";
        mailError.value = "";
    }
}



const container = reactive({
    active: false,
    active1: false,
})

function accept() {
    container.active = false;
    container.active1 = false;
}

</script>

<template>
    <!-- Всплывающее окно -->
    <div>
        <div class="window" v-if="container.active">
            <div class="window-main">
            <h2 class="windowTitle">Комментарий успешно отправлен</h2>
            <button class="buttonAccept" @click="accept()">Спасибо</button>
        </div>
        <div class="window-back"></div>
    </div>
        <div class="window" v-if="container.active1">
            <div class="window-main">
            <h2 class="windowTitle">Вы успешно подписались на рассылку акций и новинок</h2>
            <button class="buttonAccept" @click="accept()">Спасибо</button>
        </div>
        <div class="window-back"></div>
    </div>
    </div>
    <div class="container">
        <!-- Контакты -->
        <div class="con-contact">
            <h3>Контакты</h3>
            <div class="contact-item">
                <p>Телефон: +7 (987) 123-45-67</p>
            </div>
            <div class="contact-item">
                <p>Адрес: г. Санкт - Петербург, ул. Программная, 19к1</p>
            </div>
            <div class="contact-item">
                <p>Почта: tehno.zone@mail.ru</p>
            </div>
            <div class="social-links">
                <p>Мы в соцсетях:</p>
                <div>
                    <a href="#"><img src="../assets/vk-brands-solid1.svg" alt="Вконтакте" class="social-icon"></a>
                    <a href="#"><img src="../assets/telegram-brands-solid1.svg" alt="Телеграм" class="social-icon"></a>
                </div>
            </div>
        </div>
        <!-- Разделитель -->
        <div class="divider"></div>
        <!-- Обратная связь -->
        <div class="con-feedback">
            <h2>Обратная связь</h2>
            <input class="comment" type="text" placeholder="Ваш комментарий..." v-model="comment">
            <button class="buttonSend" @click="send()">Отправить</button>
            <p>{{ commError }}</p>
        </div>
    </div>
    <!-- Подписка на рассылку -->
    <div class="con-follow">
        <h3>Хочу быть в курсе акций и новинок</h3>
        <input class="email" type="text" placeholder="Введите вашу почту..." v-model="email">
        <button @click="follow()">Подписаться</button>
        <p>{{ mailError }}</p>
    </div>

</template>

<style scoped>

/* Блок контактов и обратной свяязи */

.container {
    display: flex;
    justify-content: center;
    gap: 100px;
    padding: 40px;
    background-color: #f8f8f8;
    width: 1200px;
    margin: 40px auto;
    border: 2px solid #dcdcdc;
    border-radius: 15px;
    box-shadow: 0 2px 8px rgba(0, 0, 0, 0.05);
}

/* Контакты */

.con-contact {
    width: 400px;
    padding: 30px;

}

.con-contact h3 {
    font-size: 24px;
    margin-bottom: 25px;
    color: #333;
    border-bottom: 2px solid black;
    padding-bottom: 10px;
}

.contact-item {
    display: flex;
    align-items: center;
}

.contact-icon {
    width: 24px;
    height: 24px;
    margin-right: 15px;
}

.social-links {
    margin-top: 30px;
}

.social-links p {
    margin-bottom: 10px;
    font-weight: 500;
}

.social-icon {
    width: 32px;
    height: 32px;
    margin-right: 15px;
    transition: transform 0.3s;
}

/* Разделитель */

.divider{
    width: 2px;
    height: 400px;
    background-color: #d1d1d1;
}

/* Обратная связь */

.con-feedback {
    width: 450px;
    padding: 30px;
}

.con-feedback h2 {
    font-size: 24px;
    margin-bottom: 25px;
    color: #333;
    border-bottom: 2px solid black;
    padding-bottom: 10px;
}

.comment {
    width: 400px;
    padding: 12px 15px;
    border: 1px solid #ddd;
    border-radius: 10px;
    font-size: 16px;
    transition: border 0.3s;
}

.buttonSend {
    width: 150px;
    height: 40px;
    color: black;
    background-color: #f0f0f0;
    border: 1px solid #dcdcdc;
    border-radius: 10px;
    margin-top: 20px;
}

.buttonSend:hover {
    background-color: #d4d4d4;
}

/* Блок подписки на рассылку */

.con-follow{
    display: flex;
    justify-content: center;
    gap: 100px;
    padding: 40px;
    background-color: white;
    width: 1200px;
    margin: 20px auto;
    gap: 40px;
    border: 2px solid #dcdcdc;
    border-radius: 15px;
    box-shadow: 0 2px 8px rgba(0, 0, 0, 0.05);
}

.email{
    width: 200px;
    height: 40px;
    border: 1px solid #ddd;
    border-radius: 10px;
    font-size: 16px;
    transition: border 0.3s;
}

.con-follow button{
    border-radius: 10px;
    border: 1px solid #dcdcdc;
    background-color: #f0f0f0;
    color: black;
    width: 150px;
    height: 40px;
}

.con-follow button:hover{
    background-color: #d4d4d4;
}

/* Всплывающее окно подтверждения */

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

.windowTitle {
    position: relative;
    z-index: 1001;
    margin-left: 45px;
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