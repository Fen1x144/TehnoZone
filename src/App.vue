<script setup>
import { RouterLink } from 'vue-router';
import { reactive, onMounted } from 'vue';


const mainbull = reactive({
    mod1: false
})

function pon() {
    mainbull.mod1 = false
}

onMounted(() => {
    setTimeout(() => {
        mainbull.mod1 = true;
    }, 3000);
});
</script>

<template>
    <div class="box">
        <header>
            <RouterLink class="RouterLink" :to="{ name: 'main' }">
                <div class="logo-container">
                    <img class="logo" src="./assets/logo.png" alt="SIGMOAUTO">
                    <h1>ТехноZone</h1>
                </div>
            </RouterLink>
            <nav class="nav-header">
                <RouterLink class="RouterLink" :to="{ name: 'main' }">Главная</RouterLink>
                <RouterLink class="RouterLink" :to="{ name: 'catalog' }">Каталог</RouterLink>
                <RouterLink class="RouterLink" :to="{ name: 'contact' }">Контакты</RouterLink>
            </nav>
            <div class="user-actions">
                <RouterLink class="RouterLink" :to="{ name: 'basket' }">
                    <img class="icons-header" src="./assets/busket1.png" alt="">
                </RouterLink>
                <RouterLink class="RouterLink" :to="{ name: 'account' }">
                    <img class="icons-header" src="./assets/user-regular.svg" alt="">
                </RouterLink>
            </div>
        </header>
        <main>
            <RouterView />
        </main>
        <footer>
            <div class="footer-container">
                <div class="footer-logo">
                    <RouterLink class="RouterLink" :to="{ name: 'main' }">ТехноZone</RouterLink>
                </div>
                <div class="footer-divider"></div>

                <div class="footer-content">
                    <nav class="footer-nav">
                        <RouterLink class="RouterLink" :to="{ name: 'main' }">Главная</RouterLink>
                        <RouterLink class="RouterLink" :to="{ name: 'catalog' }">Каталог</RouterLink>
                        <RouterLink class="RouterLink" :to="{ name: 'contact' }">Контакты</RouterLink>
                        <RouterLink class="RouterLink" :to="{ name: 'basket' }">Корзина</RouterLink>
                        <RouterLink class="RouterLink" :to="{ name: 'account' }">Личный кабинет</RouterLink>
                    </nav>

                    <div class="footer-info">
                        <div class="copyright">© 2025 ТехноZone. Все права защищены.</div>
                    </div>
                </div>

                <div class="footer-social">
                    <a href="#"><img src="./assets/vk-brands-solid.svg" alt="Вконтакте"></a>
                    <a href="#"><img src="./assets/telegram-brands-solid.svg" alt="Телеграм"></a>
                    <a href="#"><img src="./assets/youtube-brands-solid.svg"></a>
                </div>
            </div>
        </footer>
    </div>
</template>

<style scoped>
.RouterLink {
    color: white;
    text-decoration: none;
}

.box {
    display: flex;
    flex-direction: column;
    min-height: 100vh;
}

main {
    flex-grow: 1;
}

/* Шапка */

header {
    display: flex;
    align-items: center;
    justify-content: space-between;
    padding: 1rem 2rem;
    border-bottom: 1px solid #eaeaea;
    background-color: #1f1f1f;
    position: sticky;
    top: 0;
    z-index: 100;
    border-bottom-left-radius: 20px;
    border-bottom-right-radius: 20px;
}

.icons-header{
    width: 26px;
    height: 26px;
}

.logo-container {
    display: flex;
    align-items: center;
    gap: 0.75rem;
}

h1 {
    font-size: 1.25rem;
    font-weight: 600;
    margin: 0;
    color: white;
}

.nav-header {
    display: flex;
    gap: 2rem;
}

.user-actions {
    display: flex;
    gap: 1.5rem;
}

.nav-header a.router-link-exact-active {
    color: white;
    font-weight: 700;
    transform: scale(1);
}

/* Подвал */

footer {
    background: #1f1f1f;
    padding: 24px 0;
    border-top: 3px solid #4ca1af;
    border-top-left-radius: 12px;
    border-top-right-radius: 12px;
}

.footer-container {
    margin: 10px auto;
    padding: 0 32px;
    display: flex;
    align-items: center;
    gap: 24px;
}

.footer-logo {
    font-size: 20px;
    font-weight: 600;
    color: #ffffff;
}

.footer-divider {
    width: 1px;
    height: 40px;
    background-color: #d1d1d1;
}

.footer-content {
    flex: 1;
    display: flex;
    flex-direction: column;
    gap: 8px;
}

.footer-nav {
    display: flex;
    gap: 24px;
}

.footer-nav a {
    color: #777777;
    font-size: 16px;
}


.copyright, .support {
    font-size: 12px;
    color: #777777;
    text-align: center;
}

.footer-social {
    display: flex;
    gap: 16px;
}

.footer-social img {
    width: 32px;
    height: 32px;
}

</style>