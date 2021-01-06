<template>
  <div>
    <title-bar :title-stack="titleStack" />
    <hero-bar>
      ข้อมูลชั้นฝากขัง
      <nuxt-link slot="right" to="/" class="button"> Dashboard </nuxt-link>
    </hero-bar>
    <section class="section is-main-section">
      <!-- <notification class="is-info">
        <div>
          <b-icon icon="buffer" custom-size="default" />
          <b>Sorted and paginated table.</b>&nbsp;Based on Buefy's table.
        </div>
      </notification> -->

      <card-component
        class="has-table has-mobile-sort-spaced"
        title="ชั้นฝากขัง"
        icon="account-multiple"
      >
        <!-- <clients-table-sample
          :data-url="`${$router.options.base}data-sources/clients.json`"
          :checkable="true"
        /> -->

        <precase-table
          :data-url="`http://10.1.2.32:8080/api/v1/pre_cases/sector/102`"
          :checkable="true"
        />
      </card-component>

      <hr />

      <!-- <notification class="is-info">
        <div>
          <b-icon icon="buffer" custom-size="default" />
          <b>Tightly wrapped</b> &mdash; table header becomes card header
        </div>
      </notification>

      <hr /> -->

      <!-- <notification class="is-info">
        <div>
          <b-icon icon="buffer" custom-size="default" />
          <b>Empty table.</b> When there's nothing to show
        </div>
      </notification> -->

      <!-- <card-component class="has-table">
        <clients-table-sample />
      </card-component> -->
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
