<template>
  <div>
    <title-bar :title-stack="titleStack" />
    <hero-bar>
      {{ heroTitle }}
      <nuxt-link slot="right" :to="heroRouterLinkTo" class="button">
        {{ heroRouterLinkLabel }}
      </nuxt-link>
    </hero-bar>
    <section class="section is-main-section">
      <notification class="is-info">
        <div>
          <span><b>Demo only.</b> No data will be saved/updated</span>
        </div>
      </notification>
      <tiles>
        <card-component
          :title="formCardTitle"
          icon="account-edit"
          class="tile is-child"
        >
          <form @submit.prevent="submit">
            <b-field label="ID" horizontal>
              <b-input v-model="form.id" custom-class="is-static" readonly />
            </b-field>
            <hr />
            <!-- <b-field label="Avatar" horizontal>
              <file-picker />
            </b-field> -->
            <!-- <hr /> -->
            <b-field label="Name" message="Client name" horizontal>
              <b-input
                v-model="form.sector"
                placeholder="e.g. John Doe"
                required
              />
            </b-field>
            <b-field label="Company" message="Client's company name" horizontal>
              <b-input
                v-model="form.company"
                placeholder="e.g. Berton & Steinway"
                required
              />
            </b-field>
            <b-field label="City" message="Client's city" horizontal>
              <b-input
                v-model="form.city"
                placeholder="e.g. Geoffreyton"
                required
              />
            </b-field>
            <b-field label="Created" horizontal>
              <b-datepicker
                v-model="form.created_date"
                placeholder="Click to select..."
                icon="calendar-today"
                @input="input"
              >
              </b-datepicker>
            </b-field>
            <hr />
            <b-field label="Progress" horizontal>
              <b-slider v-model="form.progress" />
            </b-field>
            <hr />
            <b-field horizontal>
              <b-button
                type="is-primary"
                :loading="isLoading"
                native-type="submit"
                >Submit</b-button
              >
            </b-field>
          </form>
        </card-component>
        <card-component
          v-if="isProfileExists"
          title="ข้อมูลชั้นฝากขัง"
          icon="account"
          class="tile is-child"
        >
          <user-avatar
            :avatar="form.avatar"
            class="image has-max-width is-aligned-center"
          />
          <hr />
          <b-field label="Name">
            <b-input :value="form.sector" custom-class="is-static" readonly />
          </b-field>
          <b-field label="Company">
            <b-input :value="form.company" custom-class="is-static" readonly />
          </b-field>
          <b-field label="City">
            <b-input :value="form.city" custom-class="is-static" readonly />
          </b-field>
          <b-field label="Created">
            <b-input
              :value="createdReadable"
              custom-class="is-static"
              readonly
            />
          </b-field>
          <hr />
          <b-field label="Progress">
            <progress
              class="progress is-small is-primary"
              :value="form.progress"
              max="100"
            >
              {{ form.progress }}
            </progress>
          </b-field>
        </card-component>
      </tiles>
    </section>
  </div>
</template>

<script>
import axios from 'axios'
import dayjs from 'dayjs'
import find from 'lodash/find'
import TitleBar from '@/components/TitleBar'
import HeroBar from '@/components/HeroBar'
import Tiles from '@/components/Tiles'
import CardComponent from '@/components/CardComponent'
// import FilePicker from '@/components/FilePicker'
import UserAvatar from '@/components/UserAvatar'
import Notification from '@/components/Notification'

export default {
  name: 'PrecaseForm',
  components: {
    UserAvatar,
    // FilePicker,
    CardComponent,
    Tiles,
    HeroBar,
    TitleBar,
    Notification,
  },
  data() {
    return {
      id: null,
      isLoading: false,
      form: this.getClearFormObject(),
      createdReadable: null,
      isProfileExists: false,
    }
  },
  computed: {
    titleStack() {
      let lastCrumb

      if (this.isProfileExists) {
        lastCrumb = this.form.sector_desc.name
      } else {
        lastCrumb = 'New client'
      }

      return ['Admin', 'ชั้นฝากขัง', lastCrumb]
    },
    heroTitle() {
      if (this.isProfileExists) {
        return this.form.name
      } else {
        return 'Create Client'
      }
    },
    heroRouterLinkTo() {
      if (this.isProfileExists) {
        return '/client'
      } else {
        return '/'
      }
    },
    heroRouterLinkLabel() {
      if (this.isProfileExists) {
        return 'New client'
      } else {
        return 'Dashboard'
      }
    },
    formCardTitle() {
      if (this.isProfileExists) {
        return 'แก้ไข ข้อมูลชั้นฝากขัง'
      } else {
        return 'New Client'
      }
    },
  },
  watch: {
    id(newValue) {
      this.isProfileExists = false

      if (!newValue) {
        this.form = this.getClearFormObject()
      } else {
        this.getData()
      }
    },
  },
  created() {
    this.getData()
  },
  methods: {
    getClearFormObject() {
      return {
        id: null,
        sector: null,
        company: null,
        city: null,
        created_date: new Date(),
        created_mm_dd_yyyy: null,
        progress: 0,
      }
    },
    getData() {
      if (this.$route.params.id) {
        axios
          .get(`http://10.1.2.32:8080/api/v1/pre_cases/sector/102`)
          .then((r) => {
            const item = find(
              r.data.data,
              (item) => item.id === parseInt(this.$route.params.id)
            )

            if (item) {
              this.isProfileExists = true
              this.form = item
              this.form.created_date = new Date(item.created_mm_dd_yyyy)
              this.createdReadable = dayjs(
                new Date(item.created_mm_dd_yyyy)
              ).format('MMM D, YYYY')
            } else {
              this.$router.push({ name: 'client.new' })
            }
          })
          .catch((e) => {
            this.$buefy.toast.open({
              message: `Error: ${e.message}`,
              type: 'is-danger',
              queue: false,
            })
          })
      }
    },
    input(v) {
      this.createdReadable = dayjs(v).format('MMM D, YYYY')
    },
    submit() {
      this.isLoading = true

      setTimeout(() => {
        this.isLoading = false

        this.$buefy.snackbar.open({
          message: 'Demo only',
          queue: false,
        })
      }, 500)
    },
  },
  head() {
    return {
      title: 'Client — Admin One Nuxt.js',
    }
  },
}
</script>
