<template>
  <div>
    <title-bar :title-stack="titleStack" />
    <hero-bar>
      ข้อมูลผู้ต้องหา
      <nuxt-link slot="right" to="/" class="button"> Dashboard </nuxt-link>
    </hero-bar>
    <section class="section is-main-section">
      <notification class="is-info">
        <div>
          <b-icon icon="buffer" custom-size="default" />
          <b>Sorted and paginated table.</b>&nbsp;Based on Buefy's table.
        </div>
      </notification>

      <card-component
        class="has-table has-mobile-sort-spaced"
        title="ผู้ต้องหา"
        icon="account-multiple"
      >
        <pre-accused-table
          :data-url="`${this.$axios.defaults.baseURL}/api/v1/before_accuses/pre_case/1332`"
          :checkable="true"
        />
      </card-component>
    </section>
  </div>
</template>

<script>
import Notification from '@/components/Notification'
// import ClientsTableSample from '@/components/ClientsTableSample'
import PreAccusedTable from '@/components/PreAccusedTable'
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
    PreAccusedTable,
    Notification,
  },
  computed: {
    titleStack() {
      return ['ชั้นฝากขัง', 'ข้อมูลผู้ต้องหา']
    },
  },
  head() {
    return {
      title: 'Tables — Admin One Nuxt.js',
    }
  },
}
</script>
