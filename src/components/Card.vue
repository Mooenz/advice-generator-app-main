<script>
import axios from 'axios';

export default {
  data() {
    return {
      advice: '',
      id: '',
      buttonRotated: false,
    };
  },
  methods: {
    rotateButton() {
      this.buttonRotated = true; // establece una propiedad en el componente para indicar que el botón está girando

      setTimeout(() => {
        this.buttonRotated = false; // desactiva la propiedad después de que se haya completado la animación

        this.getAdvice();
      }, 1000);
    },
    async getAdvice() {
      const response = await axios.get('https://api.adviceslip.com/advice');
      const data = response.data;

      this.advice = data.slip.advice;
      this.id = data.slip.id;
    },
  },
  mounted() {
    this.getAdvice();
  },
};
</script>

<template>
  <article class="card">
    <section class="card__header">
      <header>
        ADVICE <span>#{{ id }}</span>
      </header>
      <h1>"{{ advice }}"</h1>
      <picture>
        <source
          srcset="@/assets/pattern-divider-desktop.svg"
          media="(min-width:992px)"
        />
        <img src="@/assets/pattern-divider-mobile.svg" alt="Separator mobile" />
      </picture>
    </section>

    <button
      @click="rotateButton"
      class="card__btn"
      :class="{ rotate: buttonRotated }"
    ></button>
  </article>
</template>

<style>
.card {
  position: relative;
  width: 100%;
  max-width: 350px;
  min-height: 240px;
  padding: 40px 0 50px;
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
  border-radius: 8px;
  background-color: var(--darkGrayishBlue);
}

.card__header {
  position: initial;
  display: flex;
  flex-direction: column;
  gap: 16px;
  padding: 0 24px;
}

.card__header header {
  position: absolute;
  top: 40px;
  left: 50%;
  font-size: 0.625rem;
  font-weight: var(--fontWeightBold);
  color: var(--neonGreen);
  text-transform: uppercase;
  letter-spacing: 3px;
  margin-bottom: 6px;
  transform: translateX(-50%);
}

.card__header h1 {
  margin: 40px 0 20px;
  font-size: 1.25rem;
  font-weight: var(--fontWeightBold);
  color: var(--white);
  line-height: 1.5;
  word-spacing: 2px;
}

.card__header picture {
  position: absolute;
  left: 50%;
  width: 100%;
  bottom: 40px;
  padding: 0 24px;
  transform: translateX(-50%);
}

.card__header img {
  width: 100%;
  object-fit: cover;
}

.card__btn {
  position: absolute;
  left: calc(50% - 25px);
  bottom: -25px;
  width: 50px;
  height: 50px;
  background-color: var(--neonGreen);
  border-radius: 50%;
  cursor: pointer;
  border: none;
  background-image: url('@/assets/icon-dice.svg');
  background-position: center;
  background-repeat: no-repeat;
  background-size: 20px;
  transition: all 0.3s ease;
}

.card__btn:hover {
  background-color: hsl(150, 100%, 70%);
  box-shadow: 0 0 30px hsl(150, 100%, 70%);
}

.rotate {
  animation: rotate 1s linear forwards;
}

@keyframes rotate {
  0% {
    transform: rotate(0);
  }
  100% {
    transform: rotate(360deg);
  }
}
</style>
