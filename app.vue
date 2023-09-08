<template>
  <header>
    <nav>
      <ul>
        <li><NuxtLink class="link" to="/">Home</NuxtLink></li>
      </ul>
    </nav>
  </header>
  <div>
    <NuxtPage />
  </div>
</template>

<style>
.link {
  text-decoration: none;
  color: #212223;
  font-weight: 900;
}
</style>
