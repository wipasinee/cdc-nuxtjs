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
          <b-field label="ประเภทคดี" horizontal>
            <b-select
              v-model="form.casetypes"
              placeholder="เลือกประเภทคดี"
              required
            >
              <option
                v-for="(casetype, index) in casetypes"
                :key="index"
                :selected="casetype.id == form.casetype"
              >
                {{ casetype.id }}{{ casetype.name }}
              </option>
            </b-select>
          </b-field>
          <b-field label="เลขคดีดำ" horizontal>
            <b-field>
              <b-input
                v-model="form.black_abb"
                icon="account"
                placeholder="เลขคดีดำ[อักษรย่อ]*"
                name="black_abb"
                required
              />
            </b-field>
            <b-field>
              <b-input
                v-model="form.black_no"
                icon="email"
                type="email"
                placeholder="เลขคดีดำ[ลำดับ]*"
                name="black_no"
                required
              />
            </b-field>
            <b-field>
              <b-input
                v-model="form.black_year"
                icon="email"
                type="email"
                placeholder="เลขคดีดำ[ปี]*"
                name="black_year"
                required
              />
            </b-field>
          </b-field>
          <b-field label="ชื่อผู้ร้อง" message="Message subject" horizontal>
            <b-input
              v-model="form.requester_name"
              placeholder="e.g. Partnership proposal"
              name="requester_name"
              required
            />
          </b-field>
          <b-field
            v-model="form.comment"
            label="หมายเหตุ"
            message="Your question. Max 255 characters"
            name="comment"
            horizontal
          >
            <b-input
              v-model="form.question"
              type="textarea"
              placeholder="Explain how we can help you"
              maxlength="255"
              required
            />
          </b-field>
          <b-field horizontal>
            <b-field grouped>
              <div class="control">
                <b-button native-type="submit" type="is-primary"
                  >Submit</b-button
                >
              </div>
            </b-field>
          </b-field>
        </form>
      </card-component>
    </section>
  </div>
</template>

<script>
// import { mapState } from 'vuex'
import axios from 'axios'
import dayjs from 'dayjs'
import find from 'lodash/find'
import TitleBar from '@/components/TitleBar'
import HeroBar from '@/components/HeroBar'
// import Tiles from '@/components/Tiles'
import CardComponent from '@/components/CardComponent'
// import RadioPicker from '@/components/RadioPicker'
// import CheckboxPicker from '@/components/CheckboxPicker'
// import FilePicker from '@/components/FilePicker'
// import UserAvatar from '@/components/UserAvatar'
// import ProfileUpdateForm from '@/components/ProfileUpdateForm'
import Notification from '@/components/Notification'
export default {
  name: 'PrecaseForm',
  components: {
    // UserAvatar,
    // ProfileUpdateForm,
    // FilePicker,
    CardComponent,
    // Tiles,
    HeroBar,
    TitleBar,
    Notification,
    // RadioPicker,
    // CheckboxPicker,
  },
  data() {
    return {
      id: null,
      isLoading: false,
      form: this.getClearFormObject(),
      createdReadable: null,
      isProfileExists: false,
      customElementsForm: {
        checkbox: [],
        radio: null,
        // switch: true,
        // file: null,
      },
    }
  },
  computed: {
    titleStack() {
      let lastCrumb

      if (this.isProfileExists) {
        lastCrumb = this.form.sector_desc.name
      } else {
        lastCrumb = 'error nameจ้า'
      }

      return ['ชั้นฝากขัง', 'ข้อมูลฝากขัง', lastCrumb]
    },
    heroTitle() {
      if (this.isProfileExists) {
        return this.form.sector_desc.name
      } else {
        return 'error nameจ้า'
      }
    },
    heroRouterLinkTo() {
      if (this.isProfileExists) {
        return '/pre_case/add'
      } else {
        return '/'
      }
    },
    heroRouterLinkLabel() {
      if (this.isProfileExists) {
        return 'เพิ่ม ข้อมูลชั้นฝากขัง'
      } else {
        return 'Dashboard'
      }
    },
    formCardTitle() {
      if (this.isProfileExists) {
        return 'แก้ไข ข้อมูลชั้นฝากขัง'
      } else {
        return 'แก้ไขข้อมูลชั้นฝากขัง'
      }
    },
    // ...mapState(['userName', 'userEmail']),
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
        casetypes: [],
        // casetypes_id:null,
        black_abb: null,
        black_no: null,
        requester_name: null,
        comment: null,
        created_date: new Date(),
        created_mm_dd_yyyy: null,
      }
    },
    getData() {
      if (this.$route.params.id) {
        axios
          .get(`${this.$axios.defaults.baseURL}/api/v1/types?name=case`)
          .then((r) => {
            console.log(r.data.data)
            this.casetypes = r.data.data
          })
          .catch((e) => {
            this.$buefy.toast.open({
              message: `Error: ${e.message}`,
              type: 'is-danger',
              queue: false,
            })
          })

        axios
          .get(`${this.$axios.defaults.baseURL}/api/v1/pre_cases/sector/102`)
          .then((r) => {
            const item = find(
              r.data.data,
              (item) => item.id === parseInt(this.$route.params.id)
            )

            if (item) {
              this.isProfileExists = true
              console.log(item)
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
