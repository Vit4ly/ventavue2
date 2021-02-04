<script>
import {email, required, minLength} from 'vuelidate/lib/validators'
import ForgotPassword from "@/components/ForgotPassword";

export default {
  name: "Modal",
  data() {
    return {
      email: '',
      password: '',
      forgot: '',
      bool: true
    }
  },
  components: {
    ForgotPassword
  },
  methods: {

    submitHandler() {

      if (this.$v.$invalid) {
        console.log('hi')
        this.$v.$touch()
        return
      }
      this.$router.push('/')
    },
    backOrForward() {
      this.bool = !this.bool
    }

  },
  computed: {
    checkMail() {
      return (/^.+@.+\..+$/igm).test(this.email);
    },
  },

  validations: {
    email: {
      email,
      required,
    },
    password: {
      required,
      minLength: minLength(8)
    },
    forgot: {
      email,
      required,
    }
  },

}
</script>

<template>
  <div class="modal-backdrop">
    <div class="modal">
      <div class="modal__header">
        <img
            :src="require('@/assets/header.png')" alt=""
        >
      </div>
<!--            <transition name="forward" mode="out-in"  key="editing">-->
      <transition name="forward" >
        <form
            class="modal__form"
            novalidate
            v-if="bool"
            @submit.prevent="submitHandler"
        >
          <div class="modal__form-title">Данные для входа</div>

          <div class="modal__form-box-inp">
            <input
                id="email"
                type="text"
                class="modal__form-inp"
                placeholder="e-mail@mail.ru"
                required
                v-model.trim="$v.email.$model"
                :class="{invalid: ($v.email.$dirty && !$v.email.required) || ($v.email.$dirty && !$v.email.email)}"
            >
            <label
                :class="{errorLabel: $v.email.required}"
                for="email"
            >
              Логин*
            </label>
            <small
                class="small-error"
                v-if="($v.email.$dirty && !$v.email.required)"
            >
              Это поле обязательно для заполнения
            </small>
            <small
                class="small-error"
                v-if="(!$v.email.email)"
            >
              Укажите корректный Email
            </small>
            <i
                v-if="($v.email.required && $v.email.email)"
                class="fal fa-check"
            ></i>
            <i
                v-else-if="($v.email.required)"
                class="fal fa-times"
                :class="{errorTimes: !$v.email.email}"
                @click="email = ''"
            ></i>
          </div>
          <div class="modal__form-box-inp">
            <input
                id="password"
                class="modal__form-inp"
                type="password"
                placeholder="**********"
                required
                v-model.trim="$v.password.$model"
                :class="{invalid: ($v.password.$dirty && !$v.password.required) || ($v.password.$dirty && !$v.password.minLength)}"

            >
            <label
                class="modal__form-label"
                for="password"
                :class="{errorLabel: $v.password.required}"
            >
              Пароль*
            </label>
            <small
                class="small-error"
                v-if="($v.password.$dirty && !$v.password.required)"
            >
              Это поле обязательно для заполнения
            </small>
            <small
                class="small-error"
                v-else-if="($v.password.$dirty && !$v.password.minLength)"
            >
              Пароль должен быть не менее 8 символов,
              сейчас он {{ password.length }}
            </small>
            <i
                v-if="($v.password.required && $v.password.minLength)"
                class="fal fa-check"
            ></i>
            <i
                v-else-if="($v.password.required)"
                class="fal fa-times"
                :class="{errorTimes: !$v.password.minLength}"
                @click="password = ''"
            ></i>
          </div>
          <div class="hr"></div>
          <div class="modal__form-button-case">
            <button
                class="modal__form-button-case-link"
                type="button"
                @click="backOrForward"
            >Не помню пароль
            </button>
            <button
                type="submit"
                class="modal__form-button-case-btn"
                :class="{activeBtn: ($v.email.required && $v.email.email) && $v.password.required && $v.password.minLength}"

            >
              Войти в систему
              <i class="fal fa-chevron-right"></i>
            </button>
          </div>
        </form>
      </transition>
      <transition name="back">
        <form novalidate class="modal__form" v-if="!bool">
          <div class="modal__form-title">Востановление пароля</div>
          <div class="modal__form-box-inp">
            <input
                id="forgot"
                type="email"
                class="modal__form-inp"
                placeholder="e-mail@mail.ru"
                required
                v-model.trim="$v.forgot.$model"
                :class="{invalid: ($v.forgot.$dirty && !$v.forgot.required) || ($v.forgot.$dirty && !$v.forgot.email)}"
            >
            <label
                :class="{errorLabel: $v.forgot.required}"
                for="forgot"
            >
              Логин или e-mail*
            </label>
            <i
                v-if="($v.forgot.required && $v.forgot.email)"
                class="fal fa-check"
            ></i>
            <i
                v-else-if="($v.forgot.required)"
                class="fal fa-times"
                :class="{errorTimes: !$v.forgot.email}"
                @click="forgot = ''"
            ></i>
            <div class="modal__form-forgot-message">
              <i class="fal fa-exclamation-circle"></i>
              <div>Пароль будет отправлено на электронную почту, к которой привязана учетная запись.</div>
            </div>
            <div class="hr"></div>
            <div class="modal__form-button-case">
              <button
                  class="modal__form-button-case-link"
                  type="button"
                  @click="backOrForward"
              >
                Назад
              </button>
              <button
                  class="modal__form-button-case-btn"
                  type="submit"
                  :class="{activeBtn: $v.forgot.required && $v.forgot.email}"
              >
                Восстановить
              </button>
            </div>
          </div>
        </form>
      </transition>
<!--            </transition>-->
    </div>
  </div>
</template>

<style lang="scss" scoped>
$colorInp: #f2f2f2;
* {
  box-sizing: border-box;
}

.modal-backdrop {
  position: fixed;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;
  background-color: rgba(0, 0, 0, 0.3);
  display: flex;
  justify-content: center;
  align-items: center;
}

.modal {
  display: flex;
  flex-direction: column;
  flex: 0 0 437px;
  max-height: 485px;
  border-radius: 6px;
  background-color: #ffffff;
  overflow: hidden;
  box-shadow: 0px 2px 10px rgba(0, 0, 0, 0.25);

  &__header {
    img {
      width: 100%;
      height: auto;
      border-radius: 6px 6px 0 0;
      z-index: 1000;
      position: relative;
    }
  }

  &__form {
    font-family: 'Roboto', sans-serif;
    font-size: 14px;
    font-weight: 400;

    margin: 20px 46px 24px;
    display: flex;
    flex-direction: column;
    //flex: 1 0 auto;
    position: relative;
  }

  &__form-box-inp {
    position: relative;
    display: flex;
    flex-direction: column;
  }

  &__form-title {
    margin-bottom: 15px;
    text-align: left;
  }

  &__form-button-case {
    display: flex;
    justify-content: space-around;
    align-items: center;
    font-weight: 500;
  }

  &__form-button-case-link {
    font-size: 13px;
    font-weight: 500;
    text-decoration: none;
    border: none;
    color: #949494;
    background-color: transparent;

    &:active,
    &:focus {
      outline: none;
    }

    &:hover {
      color: #10A7A5;;
    }
  }

  &__form-button-case-btn {
    display: flex;
    align-items: center;
    border: none;
    outline: none;
    color: #949494;
    font-size: 13px;
    font-weight: 500;
    padding: 16px 15px 17px 15px;
    border-radius: 6px;
  }

  &__form-forgot-message {
    display: flex;
    align-items: center;
    justify-content: center;
    color: #949494;
    font-size: 13px;
    font-weight: 400;
    margin-bottom: 20px;

    .fa-exclamation-circle:before {
      font-size: 28px;
      color: #a9a9a9;
      padding-right: 12px;
    }

  }
}

.fal {
  padding-left: 7px;
}

.fa-check {
  position: absolute;
  top: 20px;
  right: 15px;
  color: #48a34c;
}

.fa-times {
  position: absolute;
  top: 20px;
  right: 15px;
  color: #C4C4C4;
}

.errorTimes {
  color: #EB5757;
}

.invalid {
  border: 1px solid #EB5757;

  &:focus {
    border: 1px solid #EB5757;
  }
}

label {
  position: absolute;
  top: 16px;
  color: #8c8c8c;
  padding-left: 15px;
  line-height: 20px;
  transition: all 0.5s;
  pointer-events: none;

}

.errorLabel {
  top: 2px;
  font-size: 12px;

}

.small-error {
  color: #EB5757;

}

small {
  position: absolute;
  top: 55px;

}

input {
  width: 346px;
  height: 54px;
  margin-bottom: 25px;
  outline: none;
  border: none;
  border-radius: 6px;
  background-color: #f2f2f2;
  padding-top: 16px;
  padding-left: 15px;

  &:valid {
    padding-top: 16px;

    &:-webkit-autofill {
      -webkit-box-shadow: #f2f2f2 !important;
      background-color: #f2f2f2 !important;
    }
  }

  &::placeholder {
    color: transparent;
  }

  &:focus, :valid {
    border: 1px solid #c4c4c4;
    background-color: #fff;
    //padding-top: 16px;

    &::placeholder {
      color: #D9D9D9;
    }

    &:placeholder-shown {
      transform: translate3d(0, 0, 0);
      transition: all 0.2s ease-in-out;
    }
  }

  &:focus ~ label,
  &:valid ~ label {
    top: 2px;
    font-size: 12px;
  }
}

.hr {
  border-bottom: 1px solid #f2f2f2;
  margin-bottom: 25px;
}

.activeBtn {
  background: radial-gradient(5422.06% 1119.76% at 0% 288.54%, #3C8291 0%, #00B5AD 100%);
  color: #ffffff;
}

.forward-enter-active, .forward-leave-active {
  transition: all .5s ease-out;

}

.forward-enter, .forward-leave-to {
  transform: translateY(-300%);
}

.back-enter-active, .back-leave-active {
  transition: all 1s ease-in;
}
.back-enter {
  transform: translateY(100%);
}

.back-enter, .back-leave-to {
  transform: translateY(300%);

}
//.back-leave-active {
//  transform: translateY(300%);
//}

</style>

