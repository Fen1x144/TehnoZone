<script >
// Import Swiper Vue.js components
import { Swiper, SwiperSlide } from 'swiper/vue';

// Import Swiper styles
import 'swiper/css';

import 'swiper/css/navigation';



// import required modules
import { Navigation } from 'swiper/modules';

export default {
  components: {
    Swiper,
    SwiperSlide,
  },
  setup() {
    return {
      modules: [Navigation],
    };
  },
};
</script>
<template>
    <div class="company-info">
        <p class="logo">ТехноZone</p>
        <p class="slogan">Интернет-магазин электроники</p>
        <nav class="nav-main-btn">
            <RouterLink class="RouterLink btn" :to="{ name: 'catalog' }">Телефоны</RouterLink>
            <RouterLink class="RouterLink btn" :to="{ name: 'catalog' }">Планшеты</RouterLink>
            <RouterLink class="RouterLink btn" :to="{ name: 'catalog' }">Ноутбуки</RouterLink>
        </nav>
    </div>

    <div>
        <swiper :navigation="true" :modules="modules" class="mySwiper">
            <swiper-slide><img src="https://overclockers.ru/st/legacy/blog/363829/237179_O.png"></swiper-slide>
            <swiper-slide><img src="https://overclockers.ru/st/legacy/blog/363829/237175_O.png"></swiper-slide>
            <swiper-slide><img src="https://overclockers.ru/st/legacy/blog/363829/237171_O.png"></swiper-slide>
            <swiper-slide><img src="https://overclockers.ru/st/r/800/-/legacy/blog/363829/237180_O.png"></swiper-slide>
            <swiper-slide><img src="https://overclockers.ru/st/r/800/-/legacy/blog/363829/237172_O.png"></swiper-slide>
        </swiper>
    </div>
    <div class="about-section">
        <h2 class="about-title">О нас</h2>
        <hr>
        <div class="about-description">
            <p class="about-paragraph">Добро пожаловать в ТехноZone — ваш надежный интернет-магазин электроники! Мы специализируемся на продаже современного и качественного оборудования, которое помогает делать вашу жизнь проще и ярче. В нашем ассортименте вы найдете смартфоны, ноутбуки, гаджеты, аксессуары и многое другое от ведущих мировых брендов.</p>
            <p class="about-paragraph">Наша миссия — обеспечить каждого клиента лучшими технологиями по доступным ценам и предоставить высокий уровень сервиса. Мы ценим доверие наших покупателей и постоянно расширяем ассортимент, чтобы удовлетворить любые потребности.

Присоединяйтесь к тысячам довольных клиентов ТехноZone и открывайте новые возможности с передовыми технологиями!</p>
        </div>
    </div>    
</template>
<style scoped>

/* Блок с инфой и кнопками */

.company-info {
    width: 1000px;
    padding: 20px;
    margin: 20px auto;
    background: white;
    border-radius: 5px;
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
    height: 230px;
    margin-top: 20px;
}

.company-info p{
    text-align: center;
}

.logo {
    font-size: 50px;
    font-weight: 700;
    margin-bottom: 10px;
    text-shadow: 1px 1px 2px rgba(0, 0, 0, 0.1);
}

.slogan {
    font-size: 16px;
    color: #333;
    margin-bottom: 20px;
    line-height: 1.4;
}

.nav-main-btn { 
    display: flex;
    gap: 2rem;
}

.nav-main-btn a {
    padding: 8px 15px;
    margin-bottom: 8px;
    border-radius: 8px;
    transition: all 0.3s ease;
    font-size: 30px;
    width: 500px;
    color: black;
    text-align: center;
    text-decoration: none;
} 

.btn {
  position: relative;
  color: black;
  text-decoration: none;
}

.btn:after {
    content: '';
    position: absolute;
    left: 0;
    display: inline-block;
    width: 100%;
    border-bottom: 1px solid black;
    margin-top: 1.6em;
    transition: all .3s;
    opacity: 1;
    transform: scale(1);
}

.btn:hover {
  color: black;
  opacity: .75;
}

.btn:hover:after{
  opacity: 0;
  transform: scale(0,1);
}

/* Слайдер */

.swiper {
  width: 900px;
  height: 400px;
  border-radius: 20px;
}

.swiper-slide {
  text-align: center;
  font-size: 18px;
  background: #fff;
  display: flex;
  justify-content: center;
  align-items: center;
}

.swiper-slide img {
  display: block;
  width: 900px;
  height: 400px;
  object-fit: cover;
}


/* Блок "О нас" */

.about-section {
    width: 50%;
    margin: 20px auto;
}

.about-title {
    font-size: 28px;
    color: #333;
    text-align: center;
}

.about-description {
    font-size: 16px;
    line-height: 1.6;
    color: #444;
    text-align: left;
}

.about-features {
    margin: 20px 0;
    padding-left: 20px;
}
</style>