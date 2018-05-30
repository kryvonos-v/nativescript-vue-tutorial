<template>
  <Page class="page">
    <ActionBar class="action-bar" title="Hello world"></ActionBar>

    <StackLayout class="hello-world">
      <Label class="body" textWrap=true text="This is a hello world component, tap the button if you dare"/>

      <Button class="btn btn-primary" @tap="surprise = !surprise" text="Tap me!"/>
  </Page>
</template>

<script>
  export default {
    data () {
      return {
        surprise: true
      }
    }
  }
</script>

<style scoped>
  .logo-container {
    /* background: gold; */
  }

  .logo {
    margin-top: -30;
  }

  .hello-world {
    margin: 20;
  }

  Label {
    color: red;
  }
</style>
