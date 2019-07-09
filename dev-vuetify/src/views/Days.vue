<template>
  <v-layout column>
    <v-btn
      dark
      fab
      fixed
      bottom
      right
      @click.stop="write"
      color="deep-purple darken-4">
      <v-icon
        color="deep-purple lighten-5">
        add
      </v-icon>
    </v-btn>
    <v-flex>
      <v-card>
        <v-data-table
          :headers="headers"
          :items="data"
          :expand="expand"
          item-key="KEY">
          <template
            v-slot:items="props">
            <tr @click.stop="props.expanded = !props.expanded">
              <td style="width:100px;">{{ getDate(props.item.DATE) }}</td>
              <td>{{ props.item.TITLE }}</td>
              <td style="text-align:right;">{{ props.item.TAG }}</td>
            </tr>
          </template>
          <template
            v-slot:expand="props">
            <v-card flat>
              <v-card-text>{{ props.item.CONTS }}</v-card-text>
            </v-card>
          </template>
        </v-data-table>
      </v-card>
    </v-flex>
  </v-layout>
</template>

<script>
import _ from 'lodash'
import moment from 'moment'
import DateInfo from '../mixins/DateInfo'
import DataFilter from '../components/DataFilter'
import DaysDate from '../components/DaysDate'

export default {
  data(){
    return {
      headers: [
        { text: 'DATE', value: 'DATE' },
        { text: 'TITLE', value: 'TITLE' },
        { text: 'TAG', value: 'TAG', align: 'right' }
      ],
      expand: false,
      filter: {
        code: 'C01',
        date: null,
        tag: null,
        title: null
      }
    }
  },
  components: {
    DataFilter,
    DaysDate
  },
  mixins: [DateInfo],
  methods: {
    setDate(number){
      this.$store.commit('setDate', number)
    },
    write(){
      this.$router.push({
        name: 'write',
        params: {
          data: {
            CODE: this.filter.code,
            DATE: moment().format('YYYYMMDD')
          }
        }
      })
    }
  },
  computed: {
    date(){
      return this.$store.state.date.join('.')
    },
    data(){
      /*
      let result = {}

      _.forIn(this.$store.state.data, (item, key) => {
        result[key] = {
          date: this.getDate(key),
          week: this.getWeek(key),
          weekend: this.getWeekend(key),
          data: item
        }
      })

      return result
      */
      return this.$store.state.data
    }
  },
  watch: {
    date(value){
      this.$store.dispatch('getData', this.filter)
    }
  },
  beforeCreate(){
    this.$store.commit('setData')
  },
  mounted(){
    this.$store.dispatch('getData', this.filter)
  }
}
</script>

<style lang="scss" scoped>

</style>