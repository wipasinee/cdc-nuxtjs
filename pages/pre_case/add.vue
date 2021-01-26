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
              v-model="form.casetype"
              name="casetype"
              placeholder="เลือกประเภทคดี"
              required
            >
              <option
                v-for="(casetype, index) in casetypes"
                :key="index"
                :value="casetype.id"
              >
                {{ casetype.name }}
              </option>
            </b-select>
          </b-field>
          <b-field label="เลขคดีดำ" horizontal>
            <b-select
              name="black_abb"
              placeholder="เลือก อักษรย่อเลขคดีดำ"
              required
            >
              <option value="ผ">ผ</option>
              <option value="ฝ">ฝ</option>
            </b-select>
            <b-field>
              <b-input
                name="black_no"
                type="number"
                placeholder="เลขคดีดำ[ลำดับ]*"
                required
              />
            </b-field>
            <b-field>
              <b-input
                v-model="black_year"
                oninput="javascript: if (this.value.length > this.maxLength) this.value = this.value.slice(0, this.maxLength);"
                maxlength="4"
                type="number"
                placeholder="เลขคดีดำ[ปี]*"
                name="black_year"
                required
              />
            </b-field>
          </b-field>
          <b-field label="วันที่ฝากขัง" message="Message subject" horizontal>
            <b-input
              name="date"
              type="date"
              placeholder="e.g. Partnership proposal"
              required
            />
            <div id="app">
              <div class="center">
                <h1>Select a date</h1>
                <Datepicker v-model="date"></Datepicker>
              </div>
            </div>
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
import axios from 'axios'
import Datepicker from 'vuejs-datepicker'
import mapValues from 'lodash/mapValues'
import TitleBar from '@/components/TitleBar'
import CardComponent from '@/components/CardComponent'
import HeroBar from '@/components/HeroBar'
export default {
  name: 'Add',
  components: {
    HeroBar,
    CardComponent,
    TitleBar,
    Datepicker,
  },
  data() {
    return {
      isLoading: false,
      form: {
        id: null,
        casetypes: [],
        casetype: null,
        black_abb: null,
        black_no: null,
        black_yaer: null,
        requester_name: null,
        comment: null,
        created_date: new Date(),
        created_mm_dd_yyyy: null,
      },
      date: new Date(),
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
    getData() {
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
    },
  },
  head() {
    return {
      title: 'Forms — Admin One Nuxt.js',
    }
  },
}
</script>
