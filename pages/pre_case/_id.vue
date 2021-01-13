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
      <!-- <tiles> -->
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
          <!-- <b-field label="Name" message="Client name" horizontal>
              <b-input
                v-model="form.sector"
                placeholder="e.g. John Doe"
                required
              /> -->
          <b-field label="รายชื่อผู้ต้องหาจากชั้นพนักงานสอบสวน *" horizontal>
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

          <b-field
            label="เลขคำร้องขอฝากขัง *"
            message="Client's company name"
            horizontal
          >
            <b-input
              v-model="form.company"
              placeholder="e.g. Berton & Steinway"
              required
            />
          </b-field>
          <b-field
            label="วันแรกที่นำตัวมาฝากขัง *"
            message="Client's company name"
            horizontal
          >
            <b-input
              v-model="form.company"
              placeholder="e.g. Berton & Steinway"
              required
            />
          </b-field>
          <hr />
          <b-field
            label="ยื่นคำร้องขอปล่อยตัวชั่วคราว หรือไม่"
            class="has-check"
            horizontal
          >
            <radio-picker
              v-model="customElementsForm.radio"
              :options="{ one: 'ยื่น', two: 'ไม่ยื่น' }"
              @click="showDiv = !showDiv"
            >
            </radio-picker>
          </b-field>
          <hr />
          <div v-if="showDiv">
            <b-field
              label="ความสัมพันธ์ระหว่างผู้ต้องหากับผู้ขอปล่อยชั่วคราว (นายประกัน)"
              class="has-check"
              horizontal
            >
              <checkbox-picker
                v-model="customElementsForm.checkbox"
                :options="{
                  lorem: 'ไม่ระบุ',
                  ipsum: 'บิดา',
                  dolore: 'มารดา',
                  a: 'ภรรยา',
                  b: 'บุตร',
                  c: 'นายประกันอาชีพ',
                  d: 'ญาติ',
                  e: 'ประกันตนเอง',
                  f: 'นายจ้าง',
                }"
                type="is-primary"
              />
            </b-field>
            <b-field
              label="อนุญาตปล่อยตัวชั่วคราว หรือไม่"
              class="has-check"
              horizontal
            >
              <radio-picker
                v-model="customElementsForm.radio"
                :options="{ one: 'อนุญาต', two: 'ไม่อนุญาต' }"
              >
              </radio-picker>
            </b-field>
            <hr />
          </div>
          <b-field label="ศาลที่อนุญาต" class="has-check" horizontal>
            <radio-picker
              v-model="customElementsForm.radio"
              :options="{
                one: 'ศาลชั้นต้น',
                two: 'ศาลอุทธรณ์',
                three: 'ศาลฎีกา',
              }"
            >
            </radio-picker>
          </b-field>
          <b-field
            label="วันที่อนุญาตปล่อยตัวชั่วคราว"
            message="Client's company name"
            horizontal
          >
            <b-input
              v-model="form.company"
              placeholder="e.g. Berton & Steinway"
              required
            />
          </b-field>
          <b-field label="การสิ้นสุดสัญญาประกัน" class="has-check" horizontal>
            <radio-picker
              v-model="customElementsForm.radio"
              :options="{
                one: 'หลบหนี',
                two: 'ถอนประกัน',
                three: 'อยู๋ระหว่างประกัน',
              }"
            >
            </radio-picker>
          </b-field>
          <b-field
            label="ความสัมพันธ์ระหว่างผู้ต้องหากับผู้ขอปล่อยชั่วคราว (นายประกัน)"
            class="has-check"
            horizontal
          >
            <checkbox-picker
              v-model="customElementsForm.checkbox"
              :options="{
                lorem: 'บุคคล',
                ipsum: 'เงินสด',
                dolore: 'หลักทรัพย์',
                a: 'กรมธรรม์',
                b: 'ตำแหน่ง',
                c: 'อื่นๆ',
              }"
              type="is-primary"
            />
          </b-field>
          <hr />
          <b-field label="สถานะผู้ต้องหา" class="has-check" horizontal>
            <radio-picker
              v-model="customElementsForm.radio"
              :options="{
                one: 'หลบหนี',
                two: 'ถอนประกัน',
                three: 'อยู่ระหว่างประกัน',
              }"
            >
            </radio-picker>
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
      <!-- <card-component
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
        </card-component> -->
      <!-- </tiles> -->
      <br />
      <tiles>
        <card-component
          title="ข้อมูลการฝากขัง"
          icon="account"
          class="tile is-child"
        >
          <user-avatar class="image has-max-width is-aligned-center" />
          <hr />
          <b-field label="ชื่อ - สกุล" horizontal>
            <b-input :value="userName" custom-class="is-static" readonly />
          </b-field>
          <b-field label="เลขบัตรประจำตัวประชาชน" horizontal>
            <b-input :value="userName" custom-class="is-static" readonly />
          </b-field>
          <b-field label="เพศ" horizontal>
            <b-input :value="userName" custom-class="is-static" readonly />
          </b-field>
          <b-field label="อายุ" horizontal>
            <b-input :value="userName" custom-class="is-static" readonly />
          </b-field>
          <b-field label="สัญชาติ" horizontal>
            <b-input :value="userName" custom-class="is-static" readonly />
          </b-field>
          <hr />
          <b-field label="เลขคำร้องขอฝากขัง" horizontal>
            <b-input :value="userName" custom-class="is-static" readonly />
          </b-field>
          <b-field label="วันที่ฝากขัง" horizontal>
            <b-input :value="userName" custom-class="is-static" readonly />
          </b-field>
          <hr />
          <b-field label="ยื่นคำร้องขอปล่อยตัวชั่วคราว" horizontal>
            <b-input :value="userName" custom-class="is-static" readonly />
          </b-field>
          <b-field label="นายประกันคือ" horizontal>
            <b-input :value="userName" custom-class="is-static" readonly />
          </b-field>
          <b-field label="การอนุญาตปล่อยตัวชั่วคราว" horizontal>
            <b-input :value="userName" custom-class="is-static" readonly />
          </b-field>
          <b-field label="ศาลที่ออกคำสั่ง" horizontal>
            <b-input :value="userName" custom-class="is-static" readonly />
          </b-field>
          <b-field label="วันที่อนุญาตปล่อยตัวชั่วคราว" horizontal>
            <b-input :value="userName" custom-class="is-static" readonly />
          </b-field>
          <hr />
          <b-field label="การสิ้นสุดสัญญาประกัน" horizontal>
            <b-input :value="userName" custom-class="is-static" readonly />
          </b-field>
          <b-field label="สถานะผู้ต้องหา" horizontal>
            <b-input :value="userName" custom-class="is-static" readonly />
          </b-field>
          <b-field label="หลักประกัน" horizontal>
            <b-input :value="userName" custom-class="is-static" readonly />
          </b-field>
          <b-field label="วงเงินสัญญาประกัน" horizontal>
            <b-input :value="userName" custom-class="is-static" readonly />
          </b-field>
        </card-component>
      </tiles>
    </section>
  </div>
</template>

<script>
import { mapState } from 'vuex'
import axios from 'axios'
import dayjs from 'dayjs'
import find from 'lodash/find'
import TitleBar from '@/components/TitleBar'
import HeroBar from '@/components/HeroBar'
// import Tiles from '@/components/Tiles'
import CardComponent from '@/components/CardComponent'
import RadioPicker from '@/components/RadioPicker'
import CheckboxPicker from '@/components/CheckboxPicker'
// import FilePicker from '@/components/FilePicker'
import UserAvatar from '@/components/UserAvatar'
// import ProfileUpdateForm from '@/components/ProfileUpdateForm'
import Notification from '@/components/Notification'

export default {
  name: 'PrecaseForm',
  components: {
    UserAvatar,
    // ProfileUpdateForm,
    // FilePicker,
    CardComponent,
    // Tiles,
    HeroBar,
    TitleBar,
    Notification,
    RadioPicker,
    CheckboxPicker,
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
      showDiv: true,
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
        return 'เพิ่ม ข้อมูลชั้นฝากขัง'
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
    ...mapState(['userName', 'userEmail']),
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
