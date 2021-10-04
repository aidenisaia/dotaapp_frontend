<template>
  <div id="app">
    <div id="page-wrapper">
      <header id="header">
        <h1 id="logo"><a href="/">Home</a></h1>
        <nav id="nav">
          <ul>
            <li><a href="/posts">Posts</a></li>
            <li><a href="/builds">Builds</a></li>
            <li><a href="/builds/new">Create Build</a>
              <!-- <ul>
                <li><a href="left-sidebar.html">Left Sidebar</a></li>
                <li><a href="right-sidebar.html">Right Sidebar</a></li>
                <li><a href="no-sidebar.html">No Sidebar</a></li>
                <li>
                  <a href="#">Submenu</a>
                  <ul>
                    <li><a href="#">Option 1</a></li>
                    <li><a href="#">Option 2</a></li>
                    <li><a href="#">Option 3</a></li>
                    <li><a href="#">Option 4</a></li>
                  </ul>
                </li>
              </ul> -->
            </li>
            <li>
              <div class="dropdown">
                <button class="btn btn-primary dropdown-toggle" type="button" id="dropdownMenuButton1" data-bs-toggle="dropdown" aria-expanded="false">
                  User
                </button>
                <ul class="dropdown-menu" aria-labelledby="dropdownMenuButton1">
                  <li><a href="/login" class="button primary">Login</a></li>
                  <li><a href="/logout" class="button primary">Logout</a></li>
                  <li><a href="/signup" class="button primary">Sign Up</a></li>
                </ul>
              </div>
            </li>
          </ul>
        </nav>
      </header>
      <router-view/>
    </div>
  </div>
</template>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
}

#nav {
  padding: 30px;
}

#nav a {
  font-weight: bold;
  color: #2c3e50;
}

#nav a.router-link-exact-active {
  color: #42b983;
}

#logo {
  font-size: 25px;
}

.item_list,
.hero_list {
  width: 100%;
  display: inline-block;
  flex-flow: row nowrap;
}
.image {
  max-width: 60px;
  z-index: 0;
}
.home {
  padding-top: 8em;
}
.input_text {
  max-width: 500px;
  margin: 1.5em auto;
}

.login,
.signup {
  padding-top: 8em;
}

hr {
  max-width: 1000px;
  margin: 4em auto;
}

.dropdown-menu {
  background-color: #2c3e50;
}

.error {
  color: red;
  font-size: 14pt;
}
</style>
