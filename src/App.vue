<template lang="pug">
#app
  #sidebar
    #btn
      b-link(to='/')
        font-awesome-icon(:icon='["fas", "play-circle"]')
    #btn
      b-link(to='/list')
        font-awesome-icon(:icon='["fas", "check"]')
    #btn
      b-link(to='/settings')
        font-awesome-icon(:icon='["fas", "cog"]')
  keep-alive
    router-view
</template>
