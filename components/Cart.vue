<template>
  <div class="cart">
    <div class="cart__container">
      <h2 class="cart__title">Корзина</h2>
      <span v-if="cart.length > 0" class="cart__span"> Товары в корзине </span>
      <span v-if="cart.length === 0" class="cart__span">
        Пока что вы ничего не добавили в корзину.
      </span>
      <img
        src="/close.svg"
        alt=""
        class="card__close"
        @click="$emit('is-open-cart', false)"
      />
      <div v-for="(item, i) in cart" :key="i" class="cart__product-container">
        <CartComponent :product="item" @remove-from-cart="removeFromCart(i)" />
      </div>
      <form v-if="cart.length > 0" class="form" @submit="formSubmitted">
        <p class="form__title">Оформить заказ</p>
        <input
          class="form__input"
          type="text"
          placeholder="Ваше имя"
          pattern="([A-ЯË][а-яё]+)(-[А-ЯË][а-яё]+)?"
          required
        />
        <input
          v-model="inputPhoneModel"
          v-mask="'+7 (###) ###-##-##'"
          type="text"
          placeholder="Телефон"
          class="form__input"
          required
        />
        <input
          class="form__input"
          type="text"
          placeholder="Адрес"
          pattern="[А-Яа-я0-9.,\/\s]{10,50}"
          required
        />
        <button class="form__button" type="submit">Отправить</button>
      </form>
      <button
        v-if="cart.length === 0"
        class="cart__button"
        type="submit"
        @click="$emit('is-open-cart', false)"
      >
        Перейти к выбору
      </button>
    </div>
  </div>
</template>

<script>
import CartComponent from '@/components/CartComponent'
import { mapGetters, mapActions } from 'vuex'
export default {
  components: {
    CartComponent,
  },
  props: {
    opencart: Boolean,
  },
  data() {
    return {
      cartOpen: this.opencart,
      inputPhoneModel: '',
    }
  },
  computed: {
    ...mapGetters(['cart']),
  },
  methods: {
    ...mapActions(['removeItemFromState']),
    removeFromCart(i) {
      this.removeItemFromState(i)
    },
    formSubmitted() {
      alert('Спасибо за заказ')
    },
  },
}
</script>
<style scoped>
.cart {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(255, 255, 255, 0.5);
  display: flex;
  justify-content: flex-end;
  right: 0;
}
.cart__title {
  font-size: 32px;
  margin: 20px 0 24px 48px;
}
.cart__span {
  margin-left: 48px;
  color: #59606d;
}
.card__close {
  position: absolute;
  right: 40px;
  top: 20px;
  cursor: pointer;
  width: 32px;
  height: 32px;
}
.cart__container {
  width: 460px;
  height: fit-content;
  background-color: white;
  border-bottom: 1px solid grey;
}
.cart__product-container {
  margin-top: 16px;
}
.form__title {
  font-size: 18px;
  color: #59606d;
  text-align: left;
  margin: 32px auto auto 48px;
  max-width: 364px;
}
input {
  width: 364px;
  height: 50px;
  background: #f8f8f8;
  border-radius: 8px;
  border: none;
  margin-top: 16px;
  text-indent: 14px;
  outline: none;
}
input::placeholder {
  color: #959dad;
}
.form {
  text-align: center;
}
.form__button {
  width: 364px;
  height: 50px;
  background-color: #1f1f1f;
  border-radius: 8px;
  border: none;
  color: white;
  cursor: pointer;
  margin: 24px auto 40px auto;
}
.cart__button {
  width: 364px;
  height: 50px;
  background-color: #1f1f1f;
  cursor: pointer;
  border-radius: 8px;
  border: none;
  color: white;
  margin: 24px auto 40px 48px;
}
.form__button:hover,
.cart__button:hover {
  background-color: #59606d;
}
@media screen and (max-width: 450px) {
  .form__button,
  .cart__button {
    width: 200px;
    margin: 24px auto 40px auto;
  }
  .cart__container {
    text-align: center;
  }
  .cart__title,
  .cart__span {
    margin: 20px 0 24px 0;
  }
  input {
    width: 300px;
  }
}
</style>
