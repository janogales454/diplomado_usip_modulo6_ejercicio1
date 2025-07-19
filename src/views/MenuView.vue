<template>
    <header>
        <div class="header-container">
            <img class="img-logo" alt="Vue logo" src="../assets/logo.png"/>
            <h1 class="logo">My Vue App</h1>
            <nav>
                <router-link to="/">Home</router-link> |
                <router-link to="/about">About</router-link>
            </nav>
        </div>
    </header>
</template>

<script>
// @ is an alias to /src
export default {
  name: 'MenuView',
  components: {
    
  }
}
</script>

<style scoped lang="scss">

.header-container {
  max-width: 1100px;
  margin: 0 auto;
  padding: 1.5rem;
}

header {
  background: #44d192;
  border-bottom: 1px solid #ddd;
}

header .header-container {
  display: flex;
  justify-content: space-between;
  align-items: center;
  flex-wrap: wrap;
}

.logo {
  font-size: 1.8rem;
  font-weight: bold;
}


nav {
  display: flex;
  gap: 1rem;

  a {
    text-decoration: none;
  color: #333;
  font-weight: 500;
  }
}

.img-logo {
  width: 50px;
  height: 50px;
  margin-right: 10px;
}
</style>