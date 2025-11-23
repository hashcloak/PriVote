<script setup>

import RegularText from "@/common/RegularText.vue";
import Title from "@/common/Title.vue";
import Content from "@/common/Content.vue";
import {ref} from "vue";
import SuccessBox from "@/common/SuccessBox.vue";
import ErrorBox from "@/common/ErrorBox.vue";

const selectedCandidate = ref("");
const resultType = ref < "error" | "idle" | "success" > ("idle");
const resultMessage = ref("");
const candidates = [
  {hash: "0x4123492734", name: "Elena"},
  {hash: "0x2141242814", name: "Hernan"},
  {hash: "0x3412848193", name: "Manish"},
  {hash: "0x3488129947", name: "Alex"},
];

async function handleVote() {
  // Check that you selected a correct value.
  if (!selectedCandidate.value) {
    resultType.value = "error";
    resultMessage.value = "You did not selected any candidate";
  }

  // Check that you are a valid member of the group.

  // Submit your vote.
}

</script>

<template>
  <Content>
    <Title>Step 2: Vote</Title>
    <RegularText>
      Now, let's vote! What candidate would you like to choose:
    </RegularText>

    <select v-model="selectedCandidate">
      <option disabled value="">Select your candidate</option>
      <option v-for="option in candidates" :key="option.hash" :value="option.name">{{ option.name }}</option>
    </select>

    <button @click="handleVote">Vote</button>

    <SuccessBox :message="resultMessage" v-if="resultType === 'success'"/>
    <ErrorBox :message="resultMessage" v-else-if="resultType === 'error'"/>
  </Content>
</template>

<style scoped>
</style>