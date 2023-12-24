<template>
  <h1 class="text-3xl font-semibold text-gray-700 mb-7">{{ title }}</h1>
  <div class="">
    <form
      id="payment-form"
      class="w-96 bg-white p-9 rounded-lg shadow-md"
      @submit="handleSubmit"
    >
      <div id="card-element"></div>
      <input
        id="cardholder-name"
        placeholder="Card holder name"
        type="text"
        v-model="cardholderName"
        class="shadow mt-4 appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline"
      />
      <button
        type="submit"
        class="w-full text-center bg-blue-950 hover:bg-orange-600 p-2 rounded-lg text-white mt-4"
        :class="{ 'opacity-50': loading }"
      >
        {{ loading ? "Processing..." : "Add My Card" }}
      </button>
    </form>
  </div>
</template>

<script setup lang="ts">
import { onMounted } from "vue";
import { loadStripe } from "@stripe/stripe-js";
import type {
  StripeElements,
  StripeCardElement,
  StripeCardNumberElement,
} from "@stripe/stripe-js";

const { $toast } = useNuxtApp();

const title: string = "Transactions";
let cardholderName: string = "";
let loading = ref(false);
const stripe = await loadStripe(import.meta.env.VITE_STRIPE_KEY as string);

let elements: StripeElements;
let cardElement: StripeCardElement | StripeCardNumberElement;

onMounted(async () => {
  if (stripe) {
    elements = stripe.elements();
    cardElement = elements.create("card", {
      style: {
        base: {
          color: "#32325d",
          fontSize: "16px",
          backgroundColor: "light-gray",
          "::placeholder": {
            color: "#aab7c4",
          },
        },
        invalid: {
          color: "#fa755a",
          iconColor: "#fa755a",
        },
      },
    });
    cardElement.mount("#card-element");
  }
});

const handleSubmit = async (e: Event) => {
  e.preventDefault();
  if (!cardholderName) {
    $toast.error("Please enter card holder name");
    return;
  }
  try {
    loading.value = true;
    const { error: submitError } = await elements.submit();
    if (submitError) {
      $toast.error("error submit");
      return;
    }

    const { error } = await stripe!.createPaymentMethod({
      type: "card",
      card: cardElement,
      billing_details: {
        name: cardholderName,
      },
    });

    if (error?.type === "card_error" || error?.type === "validation_error") {
      if (error.message) {
        $toast.error(error.message);
      }
    } else {
      $toast.success("Payment was done successfully");
    }
  } catch (error) {
    console.log("error", error);
    $toast.error("Something went wrong");
  } finally {
    loading.value = false;
  }
};
</script>
