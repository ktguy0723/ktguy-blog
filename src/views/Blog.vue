<template>
  <div class="blog markdown-body">
    <router-view />
    <v-btn
      color="primary"
      dark
      to="/"
    >
      <v-icon>mdi-keyboard-backspace</v-icon> &nbsp;BACK
    </v-btn>

  </div>
</template>

<style lang="scss" scoped>
.blog {
  ::v-deep {
    pre {
      overflow-x: auto;
      background-color: rgb(246,248,250);
      border-radius: .3rem;
      padding: 1rem;
      code {
        background-color: rgb(246,248,250);
      }
    }
  }
}
</style>