<template>
  <div id="app">
    <div class="wrapper">
      <button @click="showPopup = true" class="button">Налоговый вычет</button>
      <div class="overlay" v-show="showPopup"></div>
      <div class="popup" v-show="showPopup">
        <span class="close" @click="showPopup = false">
          <svg width="18" height="18" viewBox="0 0 18 18" fill="none" xmlns="http://www.w3.org/2000/svg">
          <path d="M11.4226 9.00086L17.4771 2.94467C17.6407 2.78667 17.7712 2.59768 17.8609 2.38872C17.9507 2.17976 17.998 1.95502 17.9999 1.72761C18.0019 1.50019 17.9586 1.27466 17.8725 1.06417C17.7863 0.853686 17.6592 0.662457 17.4984 0.501645C17.3375 0.340833 17.1463 0.213657 16.9358 0.12754C16.7253 0.041423 16.4998 -0.0019115 16.2724 6.46674e-05C16.045 0.00204084 15.8202 0.0492885 15.6113 0.139051C15.4023 0.228813 15.2133 0.359291 15.0553 0.522874L8.99914 6.57735L2.94467 0.522874C2.78667 0.359291 2.59768 0.228813 2.38872 0.139051C2.17976 0.0492885 1.95502 0.00204084 1.72761 6.46674e-05C1.50019 -0.0019115 1.27466 0.041423 1.06417 0.12754C0.853686 0.213657 0.662457 0.340833 0.501645 0.501645C0.340833 0.662457 0.213657 0.853686 0.12754 1.06417C0.041423 1.27466 -0.0019115 1.50019 6.46674e-05 1.72761C0.00204084 1.95502 0.0492885 2.17976 0.139051 2.38872C0.228813 2.59768 0.359291 2.78667 0.522874 2.94467L6.57735 8.99914L0.522874 15.0553C0.359291 15.2133 0.228813 15.4023 0.139051 15.6113C0.0492885 15.8202 0.00204084 16.045 6.46674e-05 16.2724C-0.0019115 16.4998 0.041423 16.7253 0.12754 16.9358C0.213657 17.1463 0.340833 17.3375 0.501645 17.4984C0.662457 17.6592 0.853686 17.7863 1.06417 17.8725C1.27466 17.9586 1.50019 18.0019 1.72761 17.9999C1.95502 17.998 2.17976 17.9507 2.38872 17.8609C2.59768 17.7712 2.78667 17.6407 2.94467 17.4771L8.99914 11.4226L15.0553 17.4771C15.2133 17.6407 15.4023 17.7712 15.6113 17.8609C15.8202 17.9507 16.045 17.998 16.2724 17.9999C16.4998 18.0019 16.7253 17.9586 16.9358 17.8725C17.1463 17.7863 17.3375 17.6592 17.4984 17.4984C17.6592 17.3375 17.7863 17.1463 17.8725 16.9358C17.9586 16.7253 18.0019 16.4998 17.9999 16.2724C17.998 16.045 17.9507 15.8202 17.8609 15.6113C17.7712 15.4023 17.6407 15.2133 17.4771 15.0553L11.4226 8.99914V9.00086Z" fill="#EA0029"/>
          </svg>

        </span>
        <div class="title">Налоговый вычет</div>
        <div class="text">
          Используйте налоговый вычет чтобы погасить ипотеку досрочно.
          Размер налогового вычета составляет не более 13% от своего
          официального годового дохода.
        </div>
        <div class="form">
          <label for="money" class="form-label">
            <span>Ваша зарплата в месяц</span>
            <input id="money" v-model="formData.salary" @input="onType($event)" placeholder="Введите данные" class="form-input" />
            <div class="form-input__error error" v-show="formData.error">Поле обязательно для заполнения</div>
          </label>
          <button class="form-count" @click="count()">Рассчитать</button>
          <div class="form-checks" v-show="formData.showChecks">
            <div class="form-checks__title">
              Итого можете внести в качестве досрочных:
            </div>
            <label class="form-checks__item" v-for="item, i in formData.checks" :key="i">
              <input type="checkbox" v-model="item.checked" />
              <span class="form-checks__input">
                <svg width="14" height="11" viewBox="0 0 14 11" fill="none" xmlns="http://www.w3.org/2000/svg">
                  <path d="M4.45455 8.70149L1.11364 5.25373L0 6.40299L4.45455 11L14 1.14925L12.8864 0L4.45455 8.70149Z" fill="white"/>
                  </svg>
              </span>
              <div class="form-checks__text">
                <span class="digit">{{item.val}} рублей</span> в {{i + 1}} год
              </div>
            </label>
          </div>
          <div class="form-small">
            <div class="form-small__text">Что уменьшаем?</div>
            <div class="form-small__buttons" v-for="button, i in formData.type" :key="button.name">
              <button class="form-small__button" :class="button.active ? 'active' : ''" @click="onTypeClick(i)">{{button.name}}</button>
            </div>
          </div>
          <button class="button form-button">Добавить</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      showPopup: false,
      formData: {
        error: null,
        salary: null,
        checks: [],
        type: [
          {
            name: "Платеж",
            active: false
          },
          {
            name: "Срок",
            active: false
          }
        ],
        showChecks: false,
        done: false
      }
    }
  },
  methods: {
    onTypeClick(index) {
      for (let i = 0; i < this.formData.type.length; i++) {
        if (i == index) {
          this.formData.type[i].active = true;
        } else {
          this.formData.type[i].active = false;
        }
      }
    },
    count() {
      if (this.formData.error == false) { 
        this.formData.showChecks = true;
        this.formData.checks = [];
        let val = +this.formData.salary * 12 * 0.13;
        let total = 260000;
        while (total > 0) {
          if ((total - val) > 0) {
            this.$set(this.formData.checks, this.formData.checks.length, {
              val: Math.round(val),
              checked: false
            })
            total -= val;
          } else {
             this.$set(this.formData.checks, this.formData.checks.length, {
              val: Math.round(total),
              checked: false
            })
            total = 0;
          }
        }
      } else {
        this.formData.error = true;
      }
    },
    onType(event) {
      if (event.target.value.length !== 0) {
        this.formData.error = false;
      } else {
        this.formData.error = true;
      }
    }
  },
};
</script>

<style lang="scss">
* {
  box-sizing: border-box;
  font-family: "Lab Grotesque", sans-serif;
}

body {
  margin: 0;
  padding: 0;
}

.wrapper {
  position: relative;
  width: 100vw;
  height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  background: linear-gradient(
      60deg,
      #ff5e56 0%,
      #DC3131 100%
    );
}
.button {
  cursor: pointer;
  z-index: 1;
  border: 1px solid #fff;
  color: #fff;
  background: none;
  padding: 16px 32px;
  font-size: 16px;
  font-weight: 500;
  border-radius: 6px;
  box-shadow: 0 0 24px rgba(#EA0029, 0.33);
  &:hover, &:active {
    background: #fff;
    color: #000
  }
  &:disabled {
    background: #BEC5CC;
    color: #fff;
  }
}

.overlay {
  position: absolute;
  width: 100%;
  height: 100%;
  top: 0;
  left: 0;
  background: #b3b3b3
}

.popup {
  z-index: 5;
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  max-width: 552px;
  width: 100%;
  padding: 32px;
  background: #fff;
  border-radius: 30px;
  .title {
    margin-bottom: 16px;
    font-weight: 500;
    font-size: 28px;
    line-height: 40px;
  }

  .text {
    max-width: 430px;
    margin-bottom: 24px;
    font-size: 14px;
    line-height: 24px;
    color: #808080;
  }
  .close {
    position: absolute;
    top: 27px;
    right: 27px;
    cursor: pointer;
  }
}

.form {
  &-label {
    display: block;
    font-weight: 500;
    font-size: 14px;
    line-height: 24px;
    margin-bottom: 8px;
  }
  &-input {
    appearance: none;
    width: 100%;
    display: block;
    margin-bottom: 4px;
    margin-top: 8px;
    padding: 8px 10px;
    border-radius: 3px;
    font-size: 14px;
    line-height: 24px;
    border: 1px solid #bec5cc;
    transition: 0.3s all ease-out;
    &::placeholder {
      color: #bec5cc;
    }
    &:hover {
      border: 1px solid #000;
    }
    &:focus, &:focus-visible {
      outline: none;
      border: 1px solid #bec5cc;
      color: #000;
      background: none;
    }
    &:disabled {
      color: #808080;
      border: 1px solid #808080;
      &:placeholder {
        color: #808080;
      }
    }
    &__error {
      font-size: 10px;
      color: #EA0029;
      line-height: 12px;
    }
    &.error {
      color: #EA0029;
      border: 1px solid #EA0029
    }
  }
  &-count {
    cursor: pointer;
    appearance: none;
    background: none;
    border: none;
    padding: 0;
    margin-bottom: 16px;
    font-weight: 500;
    font-size: 14px;
    line-height: 24px;
    color: #EA0029;
    transition: 0.3s all ease-out;
    &:hover {
      color: #F53A31;
    }
    &:focus {
      color: #EA0029;
    }
  }
  &-checks {
    padding-bottom: 6px;
    &__title {
      font-size: 14px;
      font-weight: 500;
      line-height: 24px;
    }
    &__item {
      cursor: pointer;
      padding: 16px 0;
      border-bottom: 1px solid #DFE3E6;
      display: flex;
      align-items: center;
    }
    input {
      display: none;
    }
    &__input {
      border-radius: 6px;
      width: 20px;
      height: 20px;
      background: #fff;
      border: 1px solid #DFE3E6;
      display: block;
      margin-right: 11px;
      display: flex;
      justify-content: center;
      align-items: center;
    }
    input:disabled+&__input {
      border: #BEC5CC
    }
    input:hover+&__input {
      border: 1px solid #000;
    }
    input:checked+&__input {
      border: none;
      background: linear-gradient(
        60deg,
        #ff5e56 0%,
        #DC3131 100%
      );
    }

    &__text {
      font-size: 14px;
      line-height: 24px;
      color: #808080;
    }
    .digit {
      color: #000
    }
  }
  &-small {
    display: flex;
    align-items: center;
    margin-top: 14px;
    margin-bottom: 40px;
    &__text {
      font-size: 14px;
      font-weight: 500;
      line-height: 24px;
      margin-right: 32px;
    }
    &__button {
      cursor: pointer;
      appearance: none;
      margin-right: 16px;
      border-radius: 50px;
      padding: 6px 12px;
      font-size: 14px;
      border: none;
      line-height: 24px;
      background: #EEF0F2;
      color: #000;
      &:hover {
        background: #DFE3E6;
      }
      &.active, &:active {
        color: #fff;
        background: linear-gradient(
          60deg,
          #ff5e56 0%,
          #DC3131 100%
        );
      }
    }

  }
  &-button {
    background: linear-gradient(
        60deg,
        #ff5e56 0%,
        #DC3131 100%
      );
    border: none;
    width: 100%;
    &:active, &:hover {
      color: #fff;
      background: #EA0029;
    }
    &:disabled {
      color: #fff;
      background: #BEC5CC;
    }
  }
}

@media (max-width: 768px) {
  .popup {
    max-width: 453px;
  }
}

@media (max-width: 500px) {
  .button {
    padding: 12px 24px;
  }
  .popup {
    border-radius: 0;
    max-width: none;
    width: 100%;
    min-height: 100%;
    padding: 32px 16px 16px;
    .title {
      font-size: 18px;
      line-height: 24px;
    }
    .text {
      font-size: 12px;
      line-height: 16px;
    }
    .close {
      top: 22px;
      right: 22px;
      svg {
        width: 12px;
        height: 12px;
      }
    }
  }
  .form {
    &-input {
      margin-bottom: 8px;
    }
    &-checks {
      &__title {
        max-width: 200px;
      }
    }
    &-small {
      align-items: start;
      margin-top: 24px;
      flex-direction: column;
      &__text {
        margin-bottom: 24px;
        margin-right: 0;
      }
      &__button {
        margin-right: 8px;
      }
    }

  }
}
/* FONTS */

@font-face {
  font-family: "Lab Grotesque";
  src: url("/fonts/LabGrotesque-Regular.eot");
  src: local("Lab Grotesque Regular"), local("LabGrotesque-Regular"),
    url("/fonts/LabGrotesque-Regular.eot?#iefix") format("embedded-opentype"),
    url("/fonts/LabGrotesque-Regular.woff2") format("woff2"),
    url("/fonts/LabGrotesque-Regular.woff") format("woff"),
    url("/fonts/LabGrotesque-Regular.ttf") format("truetype");
  font-weight: normal;
  font-style: normal;
}


@font-face {
  font-family: "Lab Grotesque";
  src: url("/fonts/LabGrotesque-Medium.eot");
  src: local("Lab Grotesque Medium"), local("LabGrotesque-Medium"),
    url("/fonts/LabGrotesque-Medium.eot?#iefix") format("embedded-opentype"),
    url("/fonts/LabGrotesque-Medium.woff2") format("woff2"),
    url("/fonts/LabGrotesque-Medium.woff") format("woff"),
    url("/fonts/LabGrotesque-Medium.ttf") format("truetype");
  font-weight: 500;
  font-style: normal;
}

@font-face {
  font-family: "Lab Grotesque";
  src: url("/fonts/LabGrotesque-Bold.eot");
  src: local("Lab Grotesque Bold"), local("LabGrotesque-Bold"),
    url("/fonts/LabGrotesque-Bold.eot?#iefix") format("embedded-opentype"),
    url("/fonts/LabGrotesque-Bold.woff2") format("woff2"),
    url("/fonts/LabGrotesque-Bold.woff") format("woff"),
    url("/fonts/LabGrotesque-Bold.ttf") format("truetype");
  font-weight: bold;
  font-style: normal;
}

</style>
