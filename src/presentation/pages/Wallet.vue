<template lang="pug">
container.page.page-main
  Header
  PaymentCard(
    title="EWallet"
    subtitle="PhonePe.Paytm & more"
    :imageUrl="require('~/assets/mfi-UPI.png')"
    :showBack="true"
    @back="onClickBack"
  )
  container.card-row
    container.hover-card.card-paytm(
      @click="()=>setWallet(EWalletType.paytm)"
      :class="[walletType===EWalletType.paytm ? 'chosen-card' : '']")
      img(:src="require('~/assets/mfi-paytm.png')")
      span paytm
    container.hover-card.card-amazon(
      @click="()=>setWallet(EWalletType.amazon)"
      :class="[walletType===EWalletType.amazon ? 'chosen-card' : '']")
      img(:src="require('~/assets/mfi-amazon-pay.png')")
      span amazon
    container.hover-card.card-gpay(
      @click="()=>setWallet(EWalletType.gpay)"
      :class="[walletType===EWalletType.gpay ? 'chosen-card' : '']")
      img(:src="require('~/assets/mfi-google-pay.png')")
      span gpay
  container.card-row
    container.hover-card.card-ola(
      @click="()=>setWallet(EWalletType.ola)"
      :class="[walletType===EWalletType.ola ? 'chosen-card' : '']")
      img(:src="require('~/assets/mfi-ola-money.png')")
      span ola money
    container.hover-card.card-jio(
      @click="()=>setWallet(EWalletType.jio)"
      :class="[walletType===EWalletType.jio ? 'chosen-card' : '']")
      img(:src="require('~/assets/mfi-jio-money.png')")
      span jio money
    container.hover-card.card-free(
      @click="()=>setWallet(EWalletType.freecharge)"
      :class="[walletType===EWalletType.freecharge ? 'chosen-card' : '']")
      img(:src="require('~/assets/mfi-freecharge.png')")
      span freecharge

  container.page-main-submit
    button PAY ₹500.00

</template>

<script lang="ts">
import { defineComponent, reactive, ref, toRefs } from "vue";
import Container from "~/presentation/components/Container.vue";
import mainStore, { EWalletType } from "~/service/store";
import PaymentCard from "~/presentation/components/PaymentCard.vue";
import Header from "~/presentation/components/Header.vue";

export default defineComponent({
  name: "Wallet",
  components: {
    Container,
    PaymentCard,
    Header
  },
  props: {
    invitationCode: {
      type: String
    }
  },
  emit: ["close", "route"],
  setup(props, { emit }) {
    const loading = ref(false);
    const onClickUPI = () => {
      mainStore.routeTo("UPI");
    };
    const onClickWallet = () => {
      mainStore.routeTo("Wallet");
    };
    const onClickBack = () => {
      console.log("back");
      mainStore.routeBack();
    };
    /** 儲存參數 */
    return {
      ...toRefs(mainStore.state),
      loading,
      onClickUPI,
      onClickWallet,
      onClickBack,
      EWalletType,
      setWallet(t: EWalletType) {
        console.log(t);
        mainStore.setWalletType(t);
      }
    };
  }
});
</script>

<style lang="scss" scoped>
.page {
  .title {
    @apply text-left font-Lexend font-bold text-dark;
  }
  .subtitle {
    @apply text-left text-label;
  }

  @apply h-full relative;
  height: 100vh;
  width: 375px;

  &-main {
    // todo: remove this
    @apply px-3;

    &-submit {
      height: 4.4rem;
      padding: 1rem 2.2rem;
      margin-top: 0.8rem;
      button {
        @apply bg-primary text-white font-bold text-center items-center rounded w-full h-full;
        &:hover {
          @apply bg-primary-light;
        }
      }
    }
  }

  //&-main::after{
  //  content: "";
  //  background-image: url("src/assets/Main-EWallet.png");
  //  background-size: contain;
  //  background-repeat-x: no-repeat;
  //  background-position-x: center;
  //  opacity: 0.5;
  //  top: -23px;
  //  left: 0;
  //  bottom: 0;
  //  right: 0;
  //  position: absolute;
  //  z-index: -10;
  //}

  .card {
    @apply flex-1 flex items-center h-full;
    &-row {
      @apply flex flex-row w-full;
      height: 3.6rem;
      margin-bottom: 0.2rem;
      .hover-card {
        @apply flex-1 flex items-center h-full;
        img {
          @apply h-full;
        }
        span {
          @apply text-xs text-label font-Lexend;
        }
      }
      .chosen-card {
        @apply border border-primary #{!important};
      }
    }
  }
}
</style>
