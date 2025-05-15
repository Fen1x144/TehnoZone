<script setup>
import { reactive, ref } from 'vue';
import { useRouter } from 'vue-router'
import useUsers from "../composables/useUsers"
import { RouterLink } from 'vue-router';

const users = useUsers().users

const authorizationpolzovatel = useUsers().authorizationpolzovatel
const router = useRouter();

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
    <form>
        <h2 class="size">Авторизация пользователя</h2>
        <label for="inp3">Имя:</label><input type="text" id="inp3" placeholder="Введите имя"
            v-model="znachpolauth.nameauth">
        <p class="err">{{ errorsauth.ernameauth }}</p>
        <label for="inp4">Пароль:</label><input type="text" id="inp4" placeholder="Введите пароль"
            v-model="znachpolauth.passwordauth">
        <p class="err">{{ errorsauth.erpasswordauth }}</p><br>
        <label for="inp5">Почта:</label><input type="text" id="inp5" placeholder="Введите почту"
            v-model="znachpolauth.emailauth">
        <p class="err">{{ errorsauth.ertemailauth }}</p>
        <p class="err">{{ errorsauth.ernotpolzovatel }}</p>
        <button id="inp-auth" @click.prevent="avtoriz()">Авторизоваться</button>
        <p>Вы не зарегистрированы? <RouterLink :to="{ name: 'regestration' }" title="Личный кабинет">
                Зарегистрируйтесь</RouterLink>
        </p>
    </form>
</template>

<style scoped>

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
    border: 1px solid #dcdcdc;
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

.err {
    color: #E30613;
}
</style>