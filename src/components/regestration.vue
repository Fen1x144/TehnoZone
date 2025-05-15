<script setup>
import { reactive, ref } from 'vue';
import useUsers from "../composables/useUsers"
import { RouterLink } from 'vue-router';


const users = useUsers().users

const authorizationpolzovatel = useUsers().authorizationpolzovatel



const znachpol = reactive({
    name: null,
    password: null,
    email: null,
})

const regular = reactive({
    reg1: /^[А-ЯЁA-Z][А-Яа-яЁёA-Za-z]{3,9}$/,
    reg2: /^[а-яА-ЯёЁa-zA-Z0-9!?]{4,10}$/,
    reg3: /^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}$/,
})



const errors = reactive({
    ername: '',
    erpassword: '',
    eremail: '',
})

function zareg() {

    if (!znachpol.name) {
        errors.ername = 'Поле не может быть пустым';
    } else if (!regular.reg1.test(znachpol.name)) {
        errors.ername = 'Неверный ввод. Имя должно быть от 4 до 10 символов, начинаться с заглавной буквы и содержать только буквы (русские или английские).';
    } else {
        errors.ername = '';
    }


    if (!znachpol.password) {
        errors.erpassword = 'Поле не может быть пустым';
    } else if (!regular.reg2.test(znachpol.password)) {
        errors.erpassword = 'Неверный ввод. Пароль должен содержать от 4 до 10 символов: буквы (русские/английские), цифры, ! или ?';
    } else {
        errors.erpassword = '';
    }


    if (!znachpol.email) {
        errors.eremail = 'Поле не может быть пустым';
    } else if (!regular.reg3.test(znachpol.email)) {
        errors.eremail = "Неверный ввод почты."
    } else {
        errors.eremail = '';
    }

    users.value.forEach(user => {
        if (znachpol.name == user.name) {
            errors.ername = "Ошибка. Уже существует пользователь с таким именем";
        }
    });

    users.value.forEach(user => {
        if (znachpol.email == user.email) {
            errors.eremail = "Ошибка. Уже существует пользователь с такой почтой";
        }
    });


    // Если ошибок нет - выполняем регистрацию
    if (!errors.ername && !errors.erpassword && !errors.eremail) {
        useUsers().addUser(znachpol.name, znachpol.password, znachpol.email)
        mainbull.mod1 = true
        znachpol.name = null
        znachpol.password = null
        znachpol.email = null
    }

}

const mainbull = reactive({
    mod1: false,
})

function pon() {
    mainbull.mod1 = false
}

</script>
<template>
    <div class="modal1" v-if="mainbull.mod1">
        <div class="modal-main">
            <h2 class="modal1h2">Успешная регистрация Аккаунта</h2>
            <RouterLink :to="{name:'authorization'}"><button class="modal1btn" @click="pon()">Перейти к авторизации</button></RouterLink>
        </div>
        <div class="modal-back"></div>
    </div>
    <form>
        <h2>Регистрация пользователя</h2>
        <label for="inp1">Имя</label><input type="text" id="inp1" placeholder="Введите имя" v-model="znachpol.name"
            title="Имя должно быть от 4 до 10 символов, начинаться с заглавной буквы и содержать только буквы (русские или английские)."><br>
        <p class="osh">{{ errors.ername }}</p>
        <label for="inp2">Пароль</label><input type="text" id="inp2" placeholder="Введите пароль"
            v-model="znachpol.password"
            title="Пароль должен содержать от 4 до 10 символов: буквы (русские/английские), цифры, ! или ?"><br>
        <p class="osh">{{ errors.erpassword }}</p><br>
        <label for="inp3">Почта</label><input type="text" id="inp3" placeholder="Введите почту"
            v-model="znachpol.email"><br>
        <p class="osh">{{ errors.eremail }}</p>
        <button id="inp-reg" @click.prevent="zareg()">Зарегистрироваться</button>
        <p>Уже есть аккаунт? <RouterLink :to="{ name: 'authorization' }" title="Личный кабинет">
                Авторизуйтесь</RouterLink>
        </p>
    </form>


</template>
<style scoped>
input {
    margin-left: 20px;
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

#inp1, #inp2, #inp3 {
    height: 30px;
    width: 250px;
    border: 1px solid #dcdcdc;
    border-radius: 5px;
}

#inp2 {
    margin-top: 20px;
}

#inp-reg {
    margin-top: 20px;
    background-color: #f0f0f0;
    width: 170px;
    height: 40px;
    border: 1px solid #dcdcdc;
    border-radius: 10px;
    transition: transform 0.3s ease;
    cursor: pointer;
    padding: 10px 20px;
}

#inp-reg:hover {
    background-color: #d4d4d4;
}

.osh {
    color: #E30613;
}


.modal1 {
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

.modal-main {
    background-color: white;
    border: 1px solid black;
    height: 250px;
    width: 500px;
}

.modal-back {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    z-index: 1000;
    background-color: white;
    opacity: 0.5;

}

.modal1h2 {
    position: relative;
    z-index: 1001;
    margin-left: 80px;
}

.modal1btn {
    position: relative;
    width: 200px;
    height: 40px;
    border-radius: 10px;
    z-index: 1002;
    background-color: #f0f0f0;
    border: 1px solid #dcdcdc;
    margin-left: 150px;
    margin-top: 40px;
}

.modal1btn:hover {
    background-color: #d4d4d4;
}
</style>