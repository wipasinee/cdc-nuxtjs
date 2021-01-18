<template>
  <div>
    <title-bar :title-stack="titleStack" />
    <hero-bar>
      เพิ่ม ข้อมูลผู้ต้องหา
      <nuxt-link slot="right" to="/" class="button"> Dashboard </nuxt-link>
    </hero-bar>
    <section class="section is-main-section">
      <card-component title="ข้อมูลผู้ต้องหา" icon="account-edit">
        <form @submit.prevent="submit">
          <b-field horizontal label="Avatar">
            <file-picker />
          </b-field>
          <hr />
          <b-field
            label="เลขบัตรประชาชน/เลขที่หนังสือเดินทาง"
            message="Client's company name"
            horizontal
          >
            <b-input placeholder="e.g. Berton & Steinway" required />
          </b-field>
          <b-field
            label="ชื่อผู้ต้องหา"
            message="Client's company name"
            horizontal
          >
            <b-input placeholder="e.g. Berton & Steinway" required />
          </b-field>
          <hr />
          <b-field label="วันจับกุม/วันฝากขัง/วันครบกำหนดฝากขัง" horizontal>
            <b-field>
              <b-input
                icon="account"
                placeholder="วันที่จับกุม"
                name="name"
                required
              />
            </b-field>
            <b-field>
              <b-input
                icon="email"
                type="email"
                placeholder="วันที่ฝากขัง"
                name="email"
                required
              />
            </b-field>
            <b-field>
              <b-input
                icon="email"
                type="email"
                placeholder="วันครบกำหนดฝากขัง"
                name="email"
                required
              />
            </b-field>
          </b-field>
          <hr />
          <b-field
            label="ยื่นคำร้องขอปล่อยตัวชั่วคราว หรือไม่"
            class="has-check"
            horizontal
          >
            <radio-picker :options="{ one: 'ยื่น', two: 'ไม่ยื่น' }">
            </radio-picker>
          </b-field>
          <hr />
          <b-field
            label="ความสัมพันธ์ระหว่างผู้ต้องหากับผู้ขอปล่อยชั่วคราว (นายประกัน)"
            class="has-check"
            horizontal
          >
            <checkbox-picker
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
            <radio-picker :options="{ one: 'อนุญาต', two: 'ไม่อนุญาต' }">
            </radio-picker>
          </b-field>
          <hr />
          <b-field label="ศาลที่อนุญาต" class="has-check" horizontal>
            <radio-picker
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
            <b-input placeholder="e.g. Berton & Steinway" required />
          </b-field>
          <b-field label="การสิ้นสุดสัญญาประกัน" class="has-check" horizontal>
            <radio-picker
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
import TitleBar from '@/components/TitleBar'
import HeroBar from '@/components/HeroBar'
import CardComponent from '@/components/CardComponent'
import RadioPicker from '@/components/RadioPicker'
import CheckboxPicker from '@/components/CheckboxPicker'
import FilePicker from '@/components/FilePicker'
export default {
  name: 'Add',
  components: {
    HeroBar,
    TitleBar,
    CardComponent,
    RadioPicker,
    CheckboxPicker,
    FilePicker,
  },
  computed: {
    titleStack() {
      return ['ชั้นฝากขัง', 'เพิ่มข้อมูลชั้นฝากขัง']
    },
  },
  head() {
    return {
      title: 'Client — Admin One Nuxt.js',
    }
  },
}
</script>
