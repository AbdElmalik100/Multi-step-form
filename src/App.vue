<template >
  <main class="d-flex py-5 align-items-center justify-content-center min-vh-100">
    <div class="container">
      <div class="box desk p-2 rounded-4 d-flex align-items-start gap-lg-5 gap-3" v-if="!mobile">
        <div class="steps p-4 py-5">
          <div class="step d-flex align-items-center gap-4 mb-3">
            <span
              :class="`rounded-pill d-flex align-items-center justify-content-center fw-bold ${steps === 'step-1' ? 'active' : ''}`">1</span>
            <div>
              <span class="step-num d-block text-uppercase">Step 1</span>
              <span class="text fw-bold text-uppercase">Your Info</span>
            </div>
          </div>
          <div class="step d-flex align-items-center gap-4 mb-3">
            <span
              :class="`rounded-pill d-flex align-items-center fw-bold justify-content-center ${steps === 'step-2' ? 'active' : ''}`">2</span>
            <div>
              <span class="step-num d-block text-uppercase">Step 2</span>
              <span class="text fw-bold text-uppercase">Select Plan</span>
            </div>
          </div>
          <div class="step d-flex align-items-center gap-4 mb-3">
            <span
              :class="`rounded-pill d-flex align-items-center fw-bold justify-content-center ${steps === 'step-3' ? 'active' : ''}`">3</span>
            <div>
              <span class="step-num d-block text-uppercase">Step 3</span>
              <span class="text fw-bold text-uppercase">Add-ons</span>
            </div>
          </div>
          <div class="step d-flex align-items-center gap-4 mb-3">
            <span
              :class="`rounded-pill d-flex align-items-center fw-bold justify-content-center ${steps === 'step-4' || steps === 'finish' ? 'active' : ''}`">4</span>
            <div>
              <span class="step-num d-block text-uppercase">Step 4</span>
              <span class="text fw-bold text-uppercase">Summary</span>
            </div>
          </div>
        </div>
        <div
          :class="`action-box w-50 flex-fill p-5 d-flex flex-column ${steps === 'finish' ? 'align-items-center justify-content-center' : 'justify-content-between'}`">
          <div class="personal-info mb-4" v-if="steps === 'step-1'">
            <h1 class="fw-bold">Personal info</h1>
            <p class="mb-4">Please provide your name, email address, and phone number.</p>
            <form id="perosnal-info">
              <div class="name mb-4">
                <div class="d-flex align-items-center justify-content-between mb-2">
                  <label>Name</label>
                  <span class="error fw-bold" v-if="validation.name.$error"
                    v-text="validation.name.$errors[0].$validator === 'required' ? 'This field is required' : 'Too Long Name'"></span>
                </div>
                <input :class="`rounded p-2 px-3 w-100 ${validation.name.$error ? 'error' : ''}`" type="text"
                  placeholder="e.g. Stephan King" v-model="personalInfo.name">
              </div>
              <div class="email mb-4">
                <div class="d-flex align-items-center justify-content-between mb-2 ">
                  <label>Email Address</label>
                  <span class="error fw-bold" v-if="validation.email.$error"
                    v-text="validation.email.$errors[0].$validator === 'required' ? 'This field is required' : 'Invalid Email Address'"></span>
                </div>
                <input :class="`rounded p-2 px-3 w-100 ${validation.email.$error ? 'error' : ''}`" type="text"
                  placeholder="e.g. stephanking@lorem.com" v-model="personalInfo.email">
              </div>
              <div class="phone mb-4">
                <div class="d-flex align-items-center justify-content-between mb-2">
                  <label>Phone Number</label>
                  <span class="error fw-bold" v-if="validation.phone.$error"
                    v-text="validation.phone.$errors[0].$validator === 'required' ? 'This field is required' : 'Invalid phone number'"></span>
                </div>
                <input :class="`rounded p-2 px-3 w-100 ${validation.phone.$error ? 'error' : ''}`" type="Phone"
                  placeholder="e.g. +1 234 567 890" v-model="personalInfo.phone">
              </div>
            </form>
          </div>
          <div class="plans" v-else-if="steps === 'step-2'">
            <h1 class="heading fw-bold">Select your plan</h1>
            <p class="sub-heading mb-4">You have the option of monthly or yearly billing.</p>
            <div class="plans-cont d-flex align-items-center gap-4">
              <div class="w-100" v-for="(plan, index) in plans" :key="index">
                <input class="d-none" :id="`${plan.name}-plan`" type="radio" name="plan" :value="index"
                  v-model="choosedPlan" :checked="index === choosedPlan">
                <label class="plan rounded p-3 w-100 text-capitalize" :for="`${plan.name}-plan`">
                  <img class="mb-5" :src="`./images/icon-${plan.name}.svg`" alt="">
                  <span class="plan-name fw-bold d-block">{{ plan.name }}</span>
                  <span class="plan-price">${{ plan.price }}/{{ time === 'monthly' ? 'mo' : 'yr' }}</span>
                  <span class="d-block free" v-if="time === 'yearly'">2 months free</span>
                </label>
              </div>
            </div>
            <div class="time mt-5 rounded p-3 w-100 d-flex align-items-center justify-content-center gap-4">
              <span :class="`text fw-bold ${time === 'monthly' ? 'active' : ''}`">Monthly</span>
              <label for="checkbox">
                <input class="d-none" id="checkbox" type="checkbox" true-value="yearly" false-value="monthly"
                  v-model="time">
                <span class="radio-butn position-relative rounded-pill d-block"></span>
              </label>
              <span :class="`text fw-bold ${time === 'yearly' ? 'active' : ''}`">Yearly</span>
            </div>
          </div>
          <div class="add-ons" v-else-if="steps === 'step-3'">
            <h1 class="heading fw-bold">Pick add-ons</h1>
            <p class="sub-heading mb-4">Add-ons help enhance your gaming experience.</p>
            <div class="addons-cont d-flex gap-3 flex-column">
              <div v-for="(addon, index) in addOns" :key="index">
                <input class="d-none" type="checkbox" :id="`checkbox-${index}`" @change="getAddon(addon)"
                  v-model="addon.choosen">
                <label :for="`checkbox-${index}`"
                  class="p-3 px-4 rounded d-flex align-items-center justify-content-between">
                  <div class="d-flex align-items-center gap-4">
                    <span class="custom-checkbox d-block"></span>
                    <div>
                      <span class="name d-block fw-bold text-capitalize">{{ addon.name }}</span>
                      <span class="desc text-capitalize">{{ addon.desc }}</span>
                    </div>
                  </div>
                  <span class="price">+${{ addon.price }}/{{ time === 'monthly' ? 'mo' : 'yr' }}</span>
                </label>
              </div>
            </div>
          </div>
          <div class="finish" v-else-if="steps === 'step-4'">
            <h1 class="heading fw-bold">Finishing up</h1>
            <p class="sub-heading mb-4">Double-check everything looks OK before confirming.</p>
            <div class="details p-3 rounded">
              <div class="plan-final mb-3 pb-3 d-flex align-items-center justify-content-between">
                <div>
                  <span class="plan-name fw-bold d-block text-capitalize">{{ infomation[1].name }} {{ time === 'monthly' ?
                    '(monthly)' : '(yearly)' }}</span>
                  <span class="change-btn text-decoration-underline" @click="steps = 'step-2'">Change</span>
                </div>
                <span class="final-price fw-bold fs-5">${{ infomation[1].price }}/{{ time === 'monthly' ? 'mo' : 'yr'
                }}</span>
              </div>
              <div class="addons-final d-flex align-item-center justify-content-between gap-3"
                v-for="(item, index) in infomation[2]" :key="index">
                <span class="addon-name text-capitalize">{{ item.name }}</span>
                <span class="addon-price">+${{ item.price }}/{{ time === 'monthly' ? 'mo' : 'yr' }}</span>
              </div>
            </div>
            <div class="total-price p-3 d-flex align-items-center justify-content-between">
              <span>Total {{ time === 'monthly' ? '(Per month)' : '(Per year)' }}</span>
              <span class="fw-bold fs-4">+${{ totalPrice }}/{{ time === 'monthly' ? 'mo' : 'yr' }}</span>
            </div>
          </div>
          <div class="thanks text-center" v-else-if="steps === 'finish'">
            <div class="check-image rounded-circle">
              <div class="circle rounded-circle bg-white">
              </div>
            </div>
            <h1 class="heading">Thank you!</h1>
            <p class="sub-heading mb-0 px-5">
              Thanks for confirming you subscription! We hope you have fun using our platform. If you ever need support,
              please feel free to email us at support@loremgaming.com.
            </p>
          </div>
          <div
            :class="`buttons d-flex align-items-center ${steps === 'step-1' ? 'justify-content-end' : 'justify-content-between'}`"
            v-if="steps !== 'finish'">
            <span class="fw-bold" v-if="steps !== 'step-1'" @click="stepBack">Go Back</span>
            <button :class="`rounded ${steps === 'step-4' ? 'confirm' : ''}`"
              v-text="steps === 'step-4' ? 'Confirm' : 'Next Step'" @click="submission"></button>
          </div>
        </div>
      </div>
      <div class="mobile box" v-if="mobile">
        <div class="steps p-4 py-5">
          <div class="step d-flex align-items-center gap-4 mb-3">
            <span
              :class="`rounded-pill d-flex align-items-center justify-content-center fw-bold ${steps === 'step-1' ? 'active' : ''}`">1</span>
          </div>
          <div class="step d-flex align-items-center gap-4 mb-3">
            <span
              :class="`rounded-pill d-flex align-items-center fw-bold justify-content-center ${steps === 'step-2' ? 'active' : ''}`">2</span>
          </div>
          <div class="step d-flex align-items-center gap-4 mb-3">
            <span
              :class="`rounded-pill d-flex align-items-center fw-bold justify-content-center ${steps === 'step-3' ? 'active' : ''}`">3</span>
          </div>
          <div class="step d-flex align-items-center gap-4 mb-3">
            <span
              :class="`rounded-pill d-flex align-items-center fw-bold justify-content-center ${steps === 'step-4' || steps === 'finish' ? 'active' : ''}`">4</span>
          </div>
        </div>
        <div
          :class="`action-box first flex-fill p-3 py-4 bg-white rounded-2 d-flex flex-column ${steps === 'finish' ? 'align-items-center justify-content-center' : 'justify-content-between'}`">
          <div class="personal-info" v-if="steps === 'step-1'">
            <h1 class="heading fw-bold">Personal info</h1>
            <p class="sub-heading mb-4">Please provide your name, email address, and phone number.</p>
            <form id="perosnal-info">
              <div class="name mb-3">
                <div class="d-flex align-items-center justify-content-between mb-2">
                  <label>Name</label>
                  <span class="error fw-bold" v-if="validation.name.$error"
                    v-text="validation.name.$errors[0].$validator === 'required' ? 'This field is required' : 'Too Long Name'"></span>
                </div>
                <input :class="`rounded p-2 px-3 w-100 ${validation.name.$error ? 'error' : ''}`" type="text"
                  placeholder="e.g. Stephan King" v-model="personalInfo.name">
              </div>
              <div class="email mb-3">
                <div class="d-flex align-items-center justify-content-between mb-2 ">
                  <label>Email Address</label>
                  <span class="error fw-bold" v-if="validation.email.$error"
                    v-text="validation.email.$errors[0].$validator === 'required' ? 'This field is required' : 'Invalid Email Address'"></span>
                </div>
                <input :class="`rounded p-2 px-3 w-100 ${validation.email.$error ? 'error' : ''}`" type="text"
                  placeholder="e.g. stephanking@lorem.com" v-model="personalInfo.email">
              </div>
              <div class="phone">
                <div class="d-flex align-items-center justify-content-between mb-2">
                  <label>Phone Number</label>
                  <span class="error fw-bold" v-if="validation.phone.$error"
                    v-text="validation.phone.$errors[0].$validator === 'required' ? 'This field is required' : 'Invalid phone number'"></span>
                </div>
                <input :class="`rounded p-2 px-3 w-100 ${validation.phone.$error ? 'error' : ''}`" type="Phone"
                  placeholder="e.g. +1 234 567 890" v-model="personalInfo.phone">
              </div>
            </form>
          </div>
          <div class="plans" v-else-if="steps === 'step-2'">
            <h1 class="heading fw-bold">Select your plan</h1>
            <p class="sub-heading mb-4">You have the option of monthly or yearly billing.</p>
            <div class="plans-cont flex-column d-flex align-items-center gap-4">
              <div class="w-100" v-for="(plan, index) in plans" :key="index">
                <input class="d-none" :id="`${plan.name}-plan`" type="radio" name="plan" :value="index"
                  v-model="choosedPlan" :checked="index === choosedPlan">
                <label class="plan rounded p-3 w-100 text-capitalize d-flex align-items-center gap-3"
                  :for="`${plan.name}-plan`">
                  <img class="" :src="`./images/icon-${plan.name}.svg`" alt="">
                  <div>
                    <span class="plan-name fw-bold d-block">{{ plan.name }}</span>
                    <span class="plan-price">${{ plan.price }}/{{ time === 'monthly' ? 'mo' : 'yr' }}</span>
                    <span class="d-block free" v-if="time === 'yearly'">2 months free</span>
                  </div>
                </label>
              </div>
            </div>
            <div class="time mt-3 rounded p-3 w-100 d-flex align-items-center justify-content-center gap-4">
              <span :class="`text fw-bold ${time === 'monthly' ? 'active' : ''}`">Monthly</span>
              <label for="checkbox">
                <input class="d-none" id="checkbox" type="checkbox" true-value="yearly" false-value="monthly"
                  v-model="time">
                <span class="radio-butn position-relative rounded-pill d-block"></span>
              </label>
              <span :class="`text fw-bold ${time === 'yearly' ? 'active' : ''}`">Yearly</span>
            </div>
          </div>
          <div class="add-ons" v-else-if="steps === 'step-3'">
            <h1 class="heading fw-bold">Pick add-ons</h1>
            <p class="sub-heading mb-4">Add-ons help enhance your gaming experience.</p>
            <div class="addons-cont d-flex gap-3 flex-column">
              <div v-for="(addon, index) in addOns" :key="index">
                <input class="d-none" type="checkbox" :id="`checkbox-${index}`" @change="getAddon(addon)"
                  v-model="addon.choosen">
                <label :for="`checkbox-${index}`" class="p-3 rounded d-flex align-items-center justify-content-between">
                  <div class="d-flex align-items-center gap-3">
                    <span class="custom-checkbox d-block"></span>
                    <div>
                      <span class="name d-block fw-bold text-capitalize">{{ addon.name }}</span>
                      <span class="desc text-capitalize">{{ addon.desc }}</span>
                    </div>
                  </div>
                  <span class="price">+${{ addon.price }}/{{ time === 'monthly' ? 'mo' : 'yr' }}</span>
                </label>
              </div>
            </div>
          </div>
          <div class="finish" v-else-if="steps === 'step-4'">
            <h1 class="heading fw-bold">Finishing up</h1>
            <p class="sub-heading mb-4">Double-check everything looks OK before confirming.</p>
            <div class="details p-3 rounded">
              <div class="plan-final mb-3 pb-3 d-flex align-items-center justify-content-between">
                <div>
                  <span class="plan-name fw-bold d-block text-capitalize">{{ infomation[1].name }} {{ time === 'monthly' ?
                    '(monthly)' : '(yearly)' }}</span>
                  <span class="change-btn text-decoration-underline" @click="steps = 'step-2'">Change</span>
                </div>
                <span class="final-price fw-bold fs-5">${{ infomation[1].price }}/{{ time === 'monthly' ? 'mo' : 'yr'
                }}</span>
              </div>
              <div class="addons-final d-flex align-item-center justify-content-between gap-3"
                v-for="(item, index) in infomation[2]" :key="index">
                <span class="addon-name text-capitalize">{{ item.name }}</span>
                <span class="addon-price">+${{ item.price }}/{{ time === 'monthly' ? 'mo' : 'yr' }}</span>
              </div>
            </div>
            <div class="total-price p-3 d-flex align-items-center justify-content-between">
              <span>Total {{ time === 'monthly' ? '(Per month)' : '(Per year)' }}</span>
              <span class="fw-bold fs-4">+${{ totalPrice }}/{{ time === 'monthly' ? 'mo' : 'yr' }}</span>
            </div>
          </div>
          <div class="thanks text-center" v-else-if="steps === 'finish'">
            <div class="check-image rounded-circle">
              <div class="circle rounded-circle bg-white">
              </div>
            </div>
            <h1 class="heading">Thank you!</h1>
            <p class="sub-heading mb-0 px-2">
              Thanks for confirming you subscription! We hope you have fun using our platform. If you ever need support,
              please feel free to email us at support@loremgaming.com.
            </p>
          </div>
        </div>
        <div class="action-box">
          <div
            :class="`buttons position-absolute p-3 d-flex bg-white align-items-center ${steps === 'step-1' ? 'justify-content-end' : 'justify-content-between'}`"
            v-if="steps !== 'finish'">
            <span class="fw-bold" v-if="steps !== 'step-1'" @click="stepBack">Go Back</span>
            <button :class="`rounded ${steps === 'step-4' ? 'confirm' : ''}`"
              v-text="steps === 'step-4' ? 'Confirm' : 'Next Step'" @click="submission"></button>
          </div>
        </div>
      </div>
    </div>
  </main>
</template>
<script setup>
import { ref, watch, computed } from 'vue';
import { useVuelidate } from '@vuelidate/core'
import { required, email, maxLength, minLength } from '@vuelidate/validators'

const steps = ref('step-1')

const mobile = ref(false)

const infomation = ref([])

const personalInfo = ref({
  name: '',
  email: '',
  phone: ''
})
const rules = {
  name: { required, maxLength: maxLength(30) },
  email: { required, email },
  phone: { required, maxLength: maxLength(15), minLength: minLength(11) }
}
const validation = useVuelidate(rules, personalInfo)


const time = ref('monthly')
const plans = ref([
  {
    name: 'arcade',
    price: 9
  },
  {
    name: 'advanced',
    price: 12
  },
  {
    name: 'pro',
    price: 15
  },
])
const choosedPlan = ref(0)


const choosedAddons = ref([])
const addOns = ref([
  {
    name: 'online services',
    desc: 'access to multiplayer games',
    price: 1,
    choosen: false
  },
  {
    name: 'larger storage',
    desc: 'extra 1TB of cloud save',
    price: 2,
    choosen: false
  },
  {
    name: 'customizable profile',
    desc: 'custom theme on your profile',
    price: 2,
    choosen: false
  },
])


const getAddon = (addon) => {
  if (addon.choosen) {
    choosedAddons.value.push(addon)
  } else {
    choosedAddons.value = choosedAddons.value.filter(el => el != addon)
  }
}

const totalPrice = computed(() => {
  let totalAddons = 0
  infomation.value[2].forEach(el => {
    console.log(el.price);
    totalAddons += el.price
  })
  return infomation.value[1].price + totalAddons
})


watch(time, newVal => {
  if (newVal === 'yearly') {
    plans.value = [
      { name: 'arcade', price: 90 },
      { name: 'advanced', price: 120 },
      { name: 'pro', price: 150 },
    ]
    addOns.value = [
      { name: 'online services', desc: 'access to multiplayer games', price: 10, choosen: false },
      { name: 'larger storage', desc: 'extra 1TB of cloud save', price: 20, choosen: false },
      { name: 'customizable profile', desc: 'custom theme on your profile', price: 20, choosen: false },
    ]
    choosedAddons.value = []
  } else {
    plans.value = [
      { name: 'arcade', price: 9 },
      { name: 'advanced', price: 12 },
      { name: 'pro', price: 15 },
    ]
    addOns.value = [
      { name: 'online services', desc: 'access to multiplayer games', price: 1, choosen: false },
      { name: 'larger storage', desc: 'extra 1TB of cloud save', price: 2, choosen: false },
      { name: 'customizable profile', desc: 'custom theme on your profile', price: 2, choosen: false },
    ]
    choosedAddons.value = []
  }
})

const submission = () => {
  validation.value.$validate()
  if (steps.value === 'step-1') {
    if (!validation.value.$error) {
      console.log("complete");
      steps.value = 'step-2'
      infomation.value = [personalInfo.value]
    }
  } else if (steps.value === 'step-2') {
    steps.value = 'step-3'
    infomation.value = [personalInfo.value, plans.value[choosedPlan.value]]
  } else if (steps.value === 'step-3') {
    steps.value = 'step-4'
    infomation.value = [personalInfo.value, plans.value[choosedPlan.value], choosedAddons.value]
  } else if (steps.value === 'step-4') {
    steps.value = 'finish'
  }
}




const stepBack = () => {
  if (steps.value === 'step-2') {
    steps.value = 'step-1'
  } else if (steps.value === 'step-3') {
    steps.value = 'step-2'
  } else if (steps.value === 'step-4') {
    steps.value = 'step-3'
  }
}

// Resizing The Window 

if (window.innerWidth <= 767) {
  mobile.value = true
} else mobile.value = false

window.addEventListener('resize', () => {
  console.log(window.innerWidth);
  if (window.innerWidth <= 767) {
    mobile.value = true
  } else mobile.value = false
})

</script>
<style lang="scss">
* {
  box-sizing: border-box;
}

@font-face {
  src: url(/fonts/Ubuntu-Regular.ttf);
  font-family: 'Ubuntu';
  font-weight: normal;
}

@font-face {
  src: url(/fonts/Ubuntu-Medium.ttf);
  font-family: 'Ubuntu';
  font-weight: 500;
}

@font-face {
  src: url(/fonts/Ubuntu-Bold.ttf);
  font-family: 'Ubuntu';
  font-weight: bold;
}

:root {
  // ### Primary
  --Marine-blue: hsl(213, 96%, 18%);
  --Purplish-blue: hsl(243, 100%, 62%);
  --Pastel-blue: hsl(228, 100%, 84%);
  --Light-blue: hsl(206, 94%, 87%);
  --Strawberry-red: hsl(354, 84%, 57%);

  // ### Neutral
  --Cool-gray: hsl(231, 11%, 63%);
  --Light-gray: hsl(229, 24%, 87%);
  --Magnolia: hsl(217, 100%, 97%);
  --Alabaster: hsl(231, 100%, 99%);
  --White: hsl(0, 0%, 100%);
}

body {
  font-family: 'Ubuntu';
  background-color: var(--Magnolia);
  min-height: 100vh;
}

ul {
  list-style: none;
  padding: 0;
  margin: 0;
}

.mobile {
  display: none;
}

main {
  .box {
    background-color: var(--White);
    box-shadow: 0 5px 20px var(--Light-gray);

    .steps {
      background-image: url(/images/bg-sidebar-desktop.svg);
      background-repeat: no-repeat;
      background-size: cover;
      width: 274px;
      height: 568px;

      .step {
        &>span {
          width: 35px;
          height: 35px;
          color: var(--White);
          border: 1px solid var(--White);

          &.active {
            background-color: var(--Light-blue);
            color: var(--Marine-blue);
            border: 1px solid var(--Light-blue);
          }
        }

        .step-num {
          font-size: 13px;
          color: var(--Cool-gray);
        }

        .text {
          font-size: 15px;
          color: var(--White);
          letter-spacing: 2px;
        }
      }
    }

    .action-box {
      height: 568px;

      .heading {
        color: var(--Marine-blue);
      }

      .sub-heading {
        color: var(--Cool-gray);
      }

      .buttons {
        span {
          color: var(--Cool-gray);
          cursor: pointer;
          transition: 0.2s;

          &:hover {
            color: var(--Marine-blue);
          }
        }

        button {
          background-color: var(--Marine-blue);
          color: var(--White);
          outline: none;
          border: none;
          height: 50px;
          width: 130px;
          cursor: pointer;
          transition: 0.2s;

          &.confirm {
            background-color: var(--Purplish-blue);

            &:hover {
              background-color: hsl(243, 100%, 78%);
            }
          }

          &:hover {
            background-color: hsl(213, 78%, 31%);
          }
        }
      }

      .personal-info {

        label {
          color: var(--Marine-blue);
        }

        span.error {
          color: var(--Strawberry-red);
        }

        input {
          border: 1px solid var(--Light-gray);
          outline: none;
          color: var(--Marine-blue);
          font-weight: bold;
          cursor: pointer;
          transition: 0.2s;

          &.error {
            border: 1px solid var(--Strawberry-red) !important;
          }

          &:focus {
            border: 1px solid var(--Purplish-blue);
          }

          &::placeholder {
            font-weight: 500;
            font-size: 15px;
            color: var(--Cool-gray);
          }
        }
      }

      .plans {
        .plans-cont {
          input:checked+.plan {
            border: 1px solid var(--Purplish-blue);
            background-color: var(--Alabaster);
          }

          .plan {
            border: 1px solid var(--Light-gray);
            cursor: pointer;
            transition: 0.2s;

            .plan-name {
              color: var(--Marine-blue);
            }

            .plan-price {
              color: var(--Cool-gray);
            }

            .free {
              color: var(--Marine-blue);
            }

            &:hover {
              border: 1px solid var(--Purplish-blue);
            }
          }
        }

        .time {
          background-color: var(--Alabaster);

          span.text {
            color: var(--Cool-gray);
            transition: 0.2s;

            &.active {
              color: var(--Marine-blue);
            }
          }

          label {
            input:checked+span::before {
              left: calc(100% - 22px);
            }

            span.radio-butn {
              width: 50px;
              height: 25px;
              background-color: var(--Marine-blue);
              cursor: pointer;

              &::before {
                content: '';
                position: absolute;
                top: 50%;
                transform: translateY(-50%);
                left: 5px;
                background-color: var(--White);
                width: 17px;
                height: 17px;
                border-radius: 50%;
                transition: 0.2s ease-in-out;
              }
            }
          }
        }
      }

      .add-ons {
        .addons-cont {
          input:checked+label {
            border: 1px solid var(--Purplish-blue);
            background-color: var(--Alabaster);
          }

          input:checked+label .custom-checkbox {
            border: 1px solid var(--Purplish-blue);
            background-color: var(--Purplish-blue);
          }

          input:checked+label .custom-checkbox::before {
            opacity: 1;
          }

          label {
            cursor: pointer;
            transition: 0.2s;
            border: 1px solid var(--Light-gray);

            .custom-checkbox {
              width: 20px;
              height: 20px;
              border: 1px solid var(--Light-gray);
              border-radius: 3px;
              position: relative;
              transition: 0.2s;

              &::before {
                content: '\f00c';
                position: absolute;
                font-family: 'Fontawesome';
                font-weight: bold;
                color: var(--White);
                font-size: 13px;
                margin-top: 1px;
                left: 50%;
                top: 50%;
                transition: 0.2s;
                opacity: 0;
                transform: translate(-50%, -50%);
              }
            }

            .name {
              color: var(--Marine-blue);
            }

            .desc {
              color: var(--Cool-gray);
            }

            .price {
              color: var(--Purplish-blue);
            }
          }
        }
      }

      .finish {
        .details {
          background-color: var(--Alabaster);

          .plan-final {
            border-bottom: 2px solid var(--Light-gray);

            .plan-name {
              color: var(--Marine-blue);
            }

            .change-btn {
              color: var(--Cool-gray);
              cursor: pointer;
            }

            .final-price {
              color: var(--Marine-blue);
            }
          }

          .addons-final {
            &:not(:last-child) {
              margin-bottom: 15px;
            }

            .addon-name {
              color: var(--Cool-gray);
            }

            .addon-price {
              color: var(--Marine-blue);
            }
          }
        }

        .total-price {
          span:first-child {
            color: var(--Cool-gray);
          }

          span:last-child {
            color: var(--Purplish-blue);
          }
        }
      }

      .thanks {
        .check-image {
          width: 80px;
          height: 80px;
          margin: auto;
          background-color: var(--Strawberry-red);
          display: flex;
          align-items: center;
          justify-content: center;
          opacity: 0.7;
          margin-bottom: 25px;
          overflow: hidden;
          position: relative;

          &::before {
            content: '';
            position: absolute;
            width: 120px;
            height: 40px;
            background-color: rgba(0, 0, 0, 0.192);
            bottom: -10px;
            left: 10px;
            border-radius: 50px;
            transform: rotate(45deg);
          }

          .circle {
            width: 40px;
            height: 40px;
            position: relative;
            z-index: 55555;

            &::before {
              content: '\f00c';
              position: absolute;
              font-family: 'fontawesome';
              font-weight: bold;
              color: var(--Strawberry-red);
              top: 50%;
              left: 50%;
              transform: translate(-50%, -50%);
              font-size: 20px;
            }
          }
        }

        h2 {
          color: var(--Marine-blue);
        }

        p {
          color: var(--Cool-gray);
        }
      }
    }
  }

  @media (max-width: 767px) {
    position: relative;

    .desk {
      display: none !important;
    }

    .mobile {
      display: block;
      background: none;
      box-shadow: none;
      min-height: 140vh;
      width: 100%;

      .steps {
        position: absolute;
        left: 0;
        top: 0;
        width: 100%;
        z-index: -1;
        height: initial;
        display: flex;
        justify-content: center;
        gap: 25px;
        align-items: center;
        background-image: url(/images/bg-sidebar-mobile.svg);
        background-repeat: no-repeat;
      }
    }

    .action-box {
      height: initial !important;

      &.first {
        position: relative !important;
        top: 60px !important;
        box-shadow: 0 5px 20px var(--Light-gray);
      }

      .addons-cont {
        .desc {
          font-size: 12px;
        }
      }
    }

    .buttons {
      left: 0;
      width: 100%;
      bottom: 0;
    }
  }
}
</style>