<script setup>
import Footer from "@/common/Footer.vue";
import RegularText from "@/common/RegularText.vue";
import Title from "@/common/Title.vue";
import Content from "@/common/Content.vue";
</script>

<template>
  <header class="main-header">
    <nav>
      <RouterLink to="/">Home</RouterLink>
      <RouterLink to="/register">Register</RouterLink>
      <RouterLink to="/vote">Vote</RouterLink>
    </nav>
  </header>

  <Content>
    <Title>ImIn</Title>
    <RegularText>
      ImIn is a privacy-preserving voting system implemented in the <a href="https://aztec.network/">Aztec Network</a>,
      in which you can do voting while protecting your identity. The privacy preserving voting is powered by a Semaphore
      implementation in the Aztec Network. BUT before you vote, we need to verify that you are a HUMAN! This is done by
      using <a href="https://zkpassport.id/">ZKPassport</a>.
    </RegularText>
  </Content>
  <main>
    <RouterView/>
  </main>

  <Footer/>
</template>

<style scoped>
.main-header {
  padding: 1rem;
  font-family: Arial, Helvetica, sans-serif;
}

nav {
  display: flex;
  gap: 1.5rem;
}

</style>
