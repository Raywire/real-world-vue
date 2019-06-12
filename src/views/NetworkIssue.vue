<template>
  <div>
    <h1>Uh-oh!</h1>
    <p>
      It looks like you are experiencing some network issues, please click the
      back button and try again
    </p>
    <router-link :to="{ name: 'event-list' }"
      >Or go back to home page</router-link
    >
  </div>
</template>
