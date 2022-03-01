/* Problem number 1 but actually many problem i had :D
In this case, I want to display the selected date for easy reading in Indonesian spelling or other DD MM YYYY eg 1 Januari 2022 using the Indonesian language pack in Quasar 'id'

But in QDate it says that a good format should be YYYY/MM/DD and sure enough, when I use a mask, I get a problem, when I use the formatDate() function it returns an undefined value in several months

After looking around for how to solve it I came across the issue from the following link: https://github.com/quasarframework/quasar/issues/3287

And produce the following solution, keep in mind this solution may be not better than other
*/

<template>
  <q-page class="text-body">
    <p class="h4">Lorem page</p>
    <p>Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.</p>
    <div class="row">
      <div class="col-3">
        <!-- another field form -->
      </div>
      <div class="col-6">
        <q-field outlined dense v-model="tanggalValue" @click.native="$refs.qTglahir.show()" label="Tanggal Lahir">
          <template v-slot:control>
            <div class="self-center full-width text-black" tabindex="0">{{tanggalLabel}}</div>
          </template>
          <template v-slot:append>
            <q-icon name="event" class="cur-pointer">
              <q-popup-proxy ref="qTglahir" transition-show="scale" transition-hide="scale">
                <q-date v-model="tanggalValue" @input="onInputTanggal" />
              </q-popup-proxy>
            </q-icon>
          </template>
        </q-field>
      </div>
      <div class="col-3">
        <!-- another field form -->
      </div>
    </div>
  </q-page>
</template>

<script>
import { date } from 'quasar'

export default {
  name: 'samplePage',
  data () {
    return {
      tanggalValue: null,
      tanggalLabel: null,
    }
  },
  methods: {
    onInputTanggal(){
      this.$refs.qTglahir.hide();
      this.tanggalLabel = date.formatDate(this.tanggalValue, 'DD MMMM YYYY'); // user-viewable format according to spelling in Indonesia
      const newDateValue = date.formatDate(this.tanggalLabel, 'YYYY-MM-DD'); // new value to store into database
      console.log("ORIGINAL FORMAT: "+this.tanggalValue); // YYYY/MM/DD => 2022/03/01
      console.log("MASKED FORMAT: "+this.tanggalLabel); // DD MMMM YYYY => 1 Maret 2022
      console.log("FORMAT TO Y-m-d: "+newDateValue); // YYYY-MM-DD => 2022-03-01
    },
  }
}
</script>
