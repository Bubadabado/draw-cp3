<template>
<div id="app">
  <div class="header">
    <!--<router-link to="/">
    </router-link>-->
    <div class="title">
      <h1>Drawing App</h1>
    </div>
    <div class="navigation">
      <router-link class="nav-button" to="/">Home</router-link> |
      <router-link class="nav-button" to="/draw">Draw</router-link> |
      <router-link class="nav-button" to="/gallery">Gallery</router-link> | 
      <router-link class="nav-button" to="/admin">Edit</router-link>
    </div>
  </div>
  <div class="content">
    <router-view />
  </div>
  <div class="footer"> 
    <a href="https://github.com/Bubadabado/draw-cp3">Github Repository</a>
  </div>
</div>
</template>

<style>
html {
  box-sizing: border-box;
}

body {
  font-family: 'Courier-new', sans-serif;
  font-size: 16px;
  background: #fff;
  padding: 0px;
  margin: 0px;
}

/* Header */
.header {
  /*display: flex;*/
  font-family:'Lucida Sans', 'Lucida Sans Regular', 'Lucida Grande', 'Lucida Sans Unicode', Geneva, Verdana, sans-serif;
  text-align: center;
  padding: 10px 100px 16px 100px;
  background-color: #441c44;
  color: #f0eeee;
  text-shadow: 0 3px 4px #220d22;
  box-shadow: 0 3px 4px rgb(105, 105, 105);
}

.nav-button {
  color: rgb(224, 224, 224);
  text-decoration: none;
}

.title {
  margin-top: 5px;
}

.title h1 {
  font-size: 40px;
}

.content {
  padding: 20px 100px;
  min-height: 500px;
}

/* Footer */
.footer {
  height: 50px;
  padding: 20px 100px 0px 100px;
  background: #e3e3e3;
  font-size: 16px;
  text-align: center;
  margin: auto;
  width: 60%;
  border-radius: 5px;
  text-decoration: none;
  color: #441c44;
}

.footer a {
  color: #000;
}

h1 {
  font-size: 20px;
}

h2 {
  font-size: 14px;
}
</style>
