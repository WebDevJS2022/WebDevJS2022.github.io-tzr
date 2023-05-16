<template>
    <li class="catalog__item">
        <a class="catalog__pic" href="#" @click.prevent="gotoPage('product', {id: product.id})">
            <img :src="product.image" alt="product.title">
        </a>

        <h3 class="catalog__title">
            <a href="#">
                {{ product.title }}
            </a>
        </h3>

        <span class="catalog__price">
            {{ product.price }} ₽
        </span>

        <ul class="colors colors--black">
            <li class="colors__item" v-for="color in product.colors" :key="color.index">
                <label class="colors__label" :for="product.color">
                    <input class="colors__radio sr-only" type="radio" :value="color.index"
                    :id="product.color">
                    <span class="colors__value" :style="{ 'background-color': color.code }">
                    </span>
                </label>
            </li>
        </ul>
    </li>
</template>

<script>
import gotoPage from '@/helpers/gotoPage';

export default {
  name: 'ProductItem',
  methods: {
    gotoPage,
  },
  props: ['product'],
};
</script>
