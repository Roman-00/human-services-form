<template>
  <div id="app">
    <router-view />
  </div>
</template>

<style lang="scss">
@import url("https://fonts.googleapis.com/css2?family=Open+Sans:ital,wght@0,300;0,400;0,500;0,600;0,700;0,800;1,300;1,400;1,500;1,600;1,700;1,800&family=Roboto:ital,wght@0,100;0,300;0,400;0,500;0,700;0,900;1,100;1,300;1,500;1,700;1,900&display=swap");

*,
*:before,
*:after {
  box-sizing: border-box;
}

*,
*:before,
*:after *:active *:focus *:hover {
  outline: 0;
  outline-offset: 0;
}

html {
  font-size: 14px;
}

body {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: "Roboto", sans-serif;
  font-size: 1rem;
}

input:focus {
  outline: none;
}

h1,
h2,
h3,
h4,
h5,
h6 {
  margin: 0;
  padding: 0;
}

.container {
  max-width: 440px;
  margin: 0 auto;
  height: 100%;
}
</style>
