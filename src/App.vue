<template>
  <div id="app">
    <header id="header">
      <h1 id="logo"><a href="/posts">Posts</a></h1>
      <nav id="nav">
        <ul>
          <li><a href="index.html">Home</a></li>
          <li>
            <a href="#">Layouts</a>
            <ul>
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
            </ul>
          </li>
          <li><a href="elements.html">Elements</a></li>
          <li><a href="#" class="button primary">Sign Up</a></li>
        </ul>
      </nav>
    </header>
    <router-view/>
  </div>
</template>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
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
</style>
