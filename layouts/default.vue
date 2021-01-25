<template>
  <div id="app">
    <nav-bar />
    <aside-menu :menu="menu" />
    <nuxt />
    <footer-bar />
  </div>
</template>

<script>
// @ is an alias to /src
import NavBar from '@/components/NavBar'
import AsideMenu from '@/components/AsideMenu'
import FooterBar from '@/components/FooterBar'

export default {
  name: 'Home',
  components: {
    FooterBar,
    AsideMenu,
    NavBar,
  },
  computed: {
    menu() {
      return [
        'General',
        [
          {
            to: '/',
            icon: 'desktop-mac',
            label: 'Dashboard',
          },
        ],
        'ชั้นฝากขัง',
        [
          {
            to: '/pre_case/list',
            label: 'ข้อมูลชั้นฝากขัง',
            icon: 'table',
          },
          {
            to: '/pre_accused/list',
            label: 'ผู้ต้องหา',
            icon: 'account-circle',
          },
          // {
          //   to: '/tables',
          //   label: 'Tables',
          //   icon: 'table',
          //   updateMark: true,
          // },
          // {
          //   to: '/forms',
          //   label: 'Forms',
          //   icon: 'square-edit-outline',
          // },
          // {
          //   to: '/profile',
          //   label: 'Profile',
          //   icon: 'account-circle',
          // },
          // {
          //   label: 'Submenus',
          //   subLabel: 'Submenus Example',
          //   icon: 'view-list',
          //   menu: [
          //     {
          //       href: '#void',
          //       label: 'Sub-item One',
          //     },
          //     {
          //       href: '#void',
          //       label: 'Sub-item Two',
          //     },
          //   ],
          // },
        ],
        'ชั้นข้อมูลคดี',
        [
          {
            to: '/case/accused',
            label: 'จำเลย',
            icon: 'mdi mdi-account-multiple',
          },
          {
            to: '/case/',
            label: 'โจทก์',
            icon: 'account-circle',
          },
          {
            to: '/case/judgement',
            label: 'ชั้นพิจารณา',
            icon: 'help-circle',
          },
          {
            to: '/case/release',
            label: 'ประกันตัว(ชั้นพิจารณา)',
            icon: 'mdi mdi-lock-reset default',
          },
          {
            to: '/case/appoint',
            label: 'สืบพยานล่วงหน้า',
            icon: 'square-edit-outline',
          },
          {
            to: '/case/',
            label: 'ผลการพิจารณา',
            icon: 'mdi mdi-view-list default',
          },
          {
            to: '/case/appeal',
            label: 'ชั้นอุทธรณ์',
            icon: 'mdi mdi-animation-outline default',
          },
          {
            to: '/case/supreme',
            label: 'ชั้นฏีกา',
            icon: 'mdi mdi-bell',
          },
          // {
          //   href: 'https://admin-one-nuxt.justboil.me',
          //   label: 'Premium Demo',
          //   icon: 'credit-card',
          // },
          // {
          //   href: 'https://justboil.me/bulma-admin-template/one-nuxt',
          //   label: 'About',
          //   icon: 'help-circle',
          // },
        ],
      ]
    },
  },
  created() {
    this.$store.commit('user', {
      name: 'John Doe',
      email: 'john@example.com',
      avatar: 'https://avatars.dicebear.com/v2/gridy/John-Doe.svg',
    })
  },
  mounted() {
    document.documentElement.classList.add('has-aside-left')
    document.documentElement.classList.add('has-aside-mobile-transition')
    document.documentElement.classList.add('has-navbar-fixed-top')
    document.documentElement.classList.add('has-aside-expanded')
  },
}
</script>
