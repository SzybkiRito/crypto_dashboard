<template>
  <router-view />
</template>

<style>
@import url('./static/variables.css');

@font-face {
  font-family: SFProRegular;
  src: url('./assets/fonts/SFPRODISPLAYREGULAR.ttf');
}

body {
  margin: 0;
  /* overflow-x: hidden; */
  background: black;
}

#app {
  font-family: SF Pro Display;
  font-size: 16px;
  font-weight: 600;
  line-height: 24px;
  letter-spacing: 0em;
  text-align: left;

  font-family: SFProRegular;
}

.options_button {
  margin-left: auto;
  background: transparent;
  border: none;
  cursor: pointer;
  margin-right: 32px;
}

.crypto_balance_button--violet {
  background: var(--violet);
  color: #EBEBF3;
}

.crypto_balance_button--white {
  background: #fff;
  color: var(--violet);
  border: 1px solid #EBEBF3;
}

.crypto_balance_button--white:hover {
  background: var(--green);
  color: white;
}

.crypto_balance_button--violet:hover {
  background: var(--green);
  color: white;
}
</style>
