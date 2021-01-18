<template>
  <div>
    <title-bar :title-stack="titleStack" />
    <hero-bar>
      เพิ่ม ข้อมูลชั้นฝากขัง
      <nuxt-link slot="right" to="/" class="button"> Dashboard </nuxt-link>
    </hero-bar>
    <section class="section is-main-section">
      <card-component title="ข้อมูลชั้นฝากขัง" icon="ballot">
        <form @submit.prevent="submit">
          <b-field label="ประเภทคดี" horizontal>
            <b-select
              v-model="form.department"
              placeholder="Select a department"
              required
            >
              <option
                v-for="(department, index) in departments"
                :key="index"
                :value="department"
              >
                {{ department }}
              </option>
            </b-select>
          </b-field>
          <b-field label="เลขคดีดำ" horizontal>
            <b-field>
              <b-input
                v-model="form.name"
                icon="account"
                placeholder="เลขคดีดำ[อักษรย่อ]*"
                name="name"
                required
              />
            </b-field>
            <b-field>
              <b-input
                v-model="form.email"
                icon="email"
                type="email"
                placeholder="เลขคดีดำ[ลำดับ]*"
                name="email"
                required
              />
            </b-field>
            <b-field>
              <b-input
                v-model="form.email"
                icon="email"
                type="email"
                placeholder="เลขคดีดำ[ปี]*"
                name="email"
                required
              />
            </b-field>
          </b-field>
          <b-field label="ชื่อผู้ร้อง" message="Message subject" horizontal>
            <b-input
              v-model="form.subject"
              placeholder="e.g. Partnership proposal"
              required
            />
          </b-field>
          <b-field
            label="หมายเหตุ"
            message="Your question. Max 255 characters"
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
              <div class="control">
                <b-button type="is-primary is-outlined" @click="reset"
                  >Reset</b-button
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
import mapValues from 'lodash/mapValues'
import TitleBar from '@/components/TitleBar'
import CardComponent from '@/components/CardComponent'
// import CheckboxPicker from '@/components/CheckboxPicker'
// import RadioPicker from '@/components/RadioPicker'
// import FilePicker from '@/components/FilePicker'
import HeroBar from '@/components/HeroBar'
export default {
  name: 'Add',
  components: {
    HeroBar,
    // FilePicker,
    // RadioPicker,
    // CheckboxPicker,
    CardComponent,
    TitleBar,
  },
  data() {
    return {
      isLoading: false,
      form: {
        name: null,
        email: null,
        phone: null,
        department: null,
        subject: null,
        question: null,
      },
      // customElementsForm: {
      //   checkbox: [],
      //   radio: null,
      //   switch: true,
      //   file: null,
      // },
      departments: ['Business Development', 'Marketing', 'Sales'],
    }
  },
  computed: {
    titleStack() {
      return ['ชั้นฝากขัง', 'เพิ่มข้อมูลชั้นฝากขัง']
    },
  },
  methods: {
    submit() {},
    reset() {
      this.form = mapValues(this.form, (item) => {
        if (item && typeof item === 'object') {
          return []
        }
        return null
      })

      this.$buefy.snackbar.open({
        message: 'Reset successfully',
        queue: false,
      })
    },
  },
  head() {
    return {
      title: 'Forms — Admin One Nuxt.js',
    }
  },
}
</script>
