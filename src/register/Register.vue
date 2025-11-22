<script setup lang="ts">
import RegularText from "@/common/RegularText.vue";
import Title from "@/common/Title.vue";
import Content from "@/common/Content.vue";
import {ZKPassport} from "@zkpassport/sdk";
import qrcode from "qrcode";
import {ref} from "vue";
import SuccessBox from "@/common/SuccessBox.vue";
import ErrorBox from "@/common/ErrorBox.vue";

const boxStatus = ref<"idle" | "success" | "error">("idle");
const resultMessage = ref("");

async function handleRegisterClick() {
  const zkPassport = new ZKPassport();
  const queryBuilder = await zkPassport.request({
        name: "imin",
        purpose: "Prove that you have a valid passport to vote.",
        scope: "valid-passport",
        logo: "/hashcloak.png",
      }
  );
  const {
    url,
    onResult,
    onError,
  } = queryBuilder.done();

  await qrcode.toCanvas(document.getElementById("canvas"), url);

  onResult(async ({uniqueIdentifier, verified, _,}) => {
    console.log("Verified:", verified);
    if (verified) {
      boxStatus.value = "success";
      resultMessage.value = "Registered. Unique identifier:" + uniqueIdentifier;
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
    <Title>Register</Title>
    <RegularText>
      First, you need to prove that you are a HUMAN. This is done by using ZKPassport. ZKPassport allows you to prove
      that you have a valid passport without revealing your private data on it.

    </RegularText>

    <button class="button" @click="handleRegisterClick">Register</button>

    <div class="image">
      <canvas id="canvas"></canvas>
    </div>

    <SuccessBox :message="resultMessage" v-if="boxStatus == 'success'"/>
    <ErrorBox :message="resultMessage" v-else-if="boxStatus == 'error'"/>
  </Content>
</template>

<style scoped>
.image {
  align-content: center;
  padding: 1rem;
}

.button {
  margin: 1rem;
}
</style>