<template lang="pug">
  nav.navbar.is-transparent
    .navbar-brand
      a.navbar-item(href='https://bulma.io')
        img(src='https://bulma.io/images/bulma-logo.png', alt='Bulma: a modern CSS framework based on Flexbox', width='112', height='28')
      .navbar-burger.burger(data-target='navbarExampleTransparentExample')
        span
        span
        span
    #navbarExampleTransparentExample.navbar-menu
      .navbar-start
        a.navbar-item(href='https://bulma.io/')
          | Home
        .navbar-item.has-dropdown.is-hoverable
          a.navbar-link
            | Menus
          .navbar-dropdown.is-boxed
            router-link(:to='{path: "/order"}')
              | 50嵐
            router-link(:to='{path: "/order"}')
              | 迷客夏
            router-link(:to='{path: "/order"}')
              | 珍煮丹
            router-link(:to='{path: "/order"}')
              | Coco
            hr.navbar-divider
            router-link(:to='{path: "/order"}')
              | 竹園
            router-link(:to='{path: "/order"}')
              | 悟饕

</template>

<script>
export default {
  data () {
    return {
    }
  }
}
</script>
