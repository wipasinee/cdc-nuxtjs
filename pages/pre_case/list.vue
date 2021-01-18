<template>
  <div>
    <title-bar :title-stack="titleStack" />
    <hero-bar>
      ข้อมูลชั้นฝากขัง
      <nuxt-link slot="right" to="/" class="button"> Dashboard </nuxt-link>
    </hero-bar>
    <section class="section is-main-section">
      <card-component
        class="has-table has-mobile-sort-spaced"
        title="ชั้นฝากขัง"
        icon="account-multiple"
      >
        <precase-table
          :data-url="`http://10.1.2.32:8080/api/v1/pre_cases/sector/102`"
          :checkable="true"
        />
      </card-component>
    </section>
  </div>
</template>

<script>
// import Notification from '@/components/Notification'
// import ClientsTableSample from '@/components/ClientsTableSample'
import PrecaseTable from '@/components/PrecaseTable'
import CardComponent from '@/components/CardComponent'
import TitleBar from '@/components/TitleBar'
import HeroBar from '@/components/HeroBar'
export default {
  name: 'List',
  components: {
    HeroBar,
    TitleBar,
    CardComponent,
    // ClientsTableSample,
    PrecaseTable,
    // Notification,
  },
  computed: {
    titleStack() {
      return ['Admin', 'ข้อมูลชั้นฝากขัง']
    },
  },
  head() {
    return {
      title: 'Tables — Admin One Nuxt.js',
    }
  },
}
</script>
