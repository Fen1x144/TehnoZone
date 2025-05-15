<script setup>
import { RouterLink } from 'vue-router';
import useProducts from '../composables/useProducts';
import { reactive } from 'vue';
import { computed } from 'vue';

const { products } = useProducts();

const filters = reactive({
    filtercategory: null,
})

const local = reactive({
    mass: products,

})

const filtersproducts = computed(() => {
    let massresult = []
    local.mass.forEach(tovar => {
        // Проверка фильтра по категории
        const categoryPassed = filters.filtercategory === null ||
            tovar.category === filters.filtercategory;

        // Если оба условия выполнены, добавляем товар
        if (categoryPassed) {
            massresult.push(tovar);
        }
    });
    return massresult
});

</script>

<template>
    <div class="catalog-box">
        <div class="filtersbox">
            <div class="filterflex">
                <h2>Фильтры</h2>
                <form class="form">
                    <div>
                        <input type="radio" id="filter5" v-model="filters.filtercategory" :value="null"><label
                            for="filter5">Нет
                            фильтра</label>
                    </div><br>
                    <div>
                        <input type="radio" id="filter6" v-model="filters.filtercategory" value="Смартфоны"><label
                            for="filter6">Смартфоны</label>
                    </div><br>
                    <div>
                        <input type="radio" id="filter7" v-model="filters.filtercategory" value="Планшеты"><label
                            for="filter7">Планшеты</label>
                    </div><br>
                    <div>
                        <input type="radio" id="filter8" v-model="filters.filtercategory" value="Ноутбуки"><label
                            for="filter8">Ноутбуки</label>
                    </div><br>
                </form>
            </div>
        </div>
        <div class="cards">
            <div class="grid">
                <RouterLink class="card" :to="{ name: 'pageproduct', params: { id: product.id.toString() } }"
                    v-for="product in filtersproducts" :key="product.id">
                    <img :src="product.imgproduct" :alt="product.title" class="imgtovar">
                    <p>{{ product.title }}</p>
                    <p>Цена: {{ product.price }} руб</p>
                </RouterLink>
            </div>
        </div>           
    </div>
</template>

<style scoped>

/* Фильтр */

.filtersbox {
    display: flex;
    justify-content: center;
}

.filterflex {
    display: flex;
}

.form {
    display: flex;
    margin-top: 25px;
    margin-left: 40px;
    gap: 10px;
}

/* Карточки товаров */

.cards {
    padding: 20px;
    margin-left: 150px;
}

.grid {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(310px, 500px));
    margin: 20px;
    gap: 40px;
}

.card {
    padding: 10px;
    background: white;
    border: 1px solid #dcdcdc;
    border-radius: 10px;
    width: 400px;
    height: 350px;
    text-decoration: none;
    color: black;
    text-align: center;
}

.imgtovar {
    width: 270px;
    height: 250px;
}
</style>