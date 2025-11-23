<script setup lang="ts">
import RegularText from "@/common/RegularText.vue";
import Title from "@/common/Title.vue";
import Content from "@/common/Content.vue";
import {ZKPassport} from "@zkpassport/sdk";
import qrcode from "qrcode";
import {ref} from "vue";
import SuccessBox from "@/common/SuccessBox.vue";
import ErrorBox from "@/common/ErrorBox.vue";
import Vote from "@/sections/Vote.vue";
import {createAztecNodeClient} from "@aztec/aztec.js/node";
import {TestWallet} from "@aztec/test-wallet/client/lazy";
import {getInitialTestAccountsData} from "@aztec/accounts/testing";
import {getPXEConfig} from "@aztec/pxe/config";
import {LeanIMTContract} from "@/sections/bindings/LeanIMT";

const boxStatus = ref<"idle" | "success" | "error">("idle");
const resultMessage = ref("");

async function handleRegisterClick() {
  boxStatus.value = "idle"
  const zkPassport = new ZKPassport();
  const queryBuilder = await zkPassport.request({
        name: "PriVote",
        purpose: "Prove that you have a valid passport to vote.",
        scope: "valid-passport",
        logo: "https://hashcloak.com/logos/hashcloak_logo_colour-logo-text.png",
      }
  );
  const {
    url,
    onResult,
    onError,
  } = queryBuilder.done();

  await qrcode.toCanvas(document.getElementById("qr-canvas"), url);

  onResult(async ({uniqueIdentifier, verified,}) => {
    console.log("Verified:", verified);
    if (verified) {
      boxStatus.value = "success";
      resultMessage.value = "We confirmed that you have a valid passport. You are ready to vote! Your unique identifier is:\n" + uniqueIdentifier;
      const node = createAztecNodeClient("https://devnet.aztec-labs.com/");
      const config = getPXEConfig();
      config.proverEnabled = true;

      const wallet = await TestWallet.create(node, config);
      console.log("wallet created...")
      const [aliceAccount, bobAccount] = await getInitialTestAccountsData();
      let alice = await wallet.createSchnorrAccount(aliceAccount.secret, aliceAccount.salt);
      const leanImt = await LeanIMTContract.deploy(wallet).send({from: aliceAccount.address})
          .deployed();
      console.log("leanImt deployed...")

    } else {
      boxStatus.value = "error";
      resultMessage.value = "Not verified.";
    }
  })

  onError(async (error) => {
    boxStatus.value = "error";
    resultMessage.value = error;
  })
}
</script>

<template>
  <Content>
    <Title>Step 1: Register</Title>
    <RegularText>
      First, you need to prove that you are a HUMAN. This is done by using ZKPassport. ZKPassport allows you to prove
      that you have a valid passport without revealing your private data on it.

    </RegularText>

    <button @click="handleRegisterClick">Register</button>

    <div class="qr">
      <canvas id="qr-canvas"></canvas>
    </div>

    <SuccessBox :message="resultMessage" v-if="boxStatus === 'success'"/>
    <ErrorBox :message="resultMessage" v-else-if="boxStatus === 'error'"/>

    <Vote v-if="boxStatus === 'success'"/>
  </Content>
</template>

<style scoped>
.qr {
  margin: auto;
  text-align: center;
  padding: 1rem;
}
</style>