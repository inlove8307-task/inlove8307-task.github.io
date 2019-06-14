<template>
  <div class="filter">
    <span class="filter-select" v-show="!disabled">
      <button @click="tagList = !tagList"><span>{{ filter.tag }}</span><i class="material-icons">keyboard_arrow_down</i></button>
      <ul v-show="tagList">
        <li
          v-for="item in tag"
          v-bind:key="item.key">
          <button @click="filter.tag = item; tagList = false">{{ item }}</button>
        </li>
      </ul>
      <!--
      <select id="FILTER_TAG" v-model="filter.tag">
        <option value="">전체</option>
        <option v-for="item in tag" v-bind:key="item.key">{{ item }}</option>
      </select>
      -->
    </span>
    <span class="filter-input" v-show="!disabled">
      <label for="FILTER_DATE">날짜</label>
      <input type="number" id="FILTER_DATE" v-model="filter.date" @keyup.enter="getData">
    </span>
    <span class="filter-input" v-show="!disabled">
      <label for="FILTER_TITLE">제목</label>
      <input type="text" id="FILTER_TITLE" v-model="filter.title" @keyup.enter="getData">
    </span>
    <span class="filter-button" v-show="!disabled">
      <button @click="getData"><i class="material-icons">search</i></button>
      <button @click="clear"><i class="material-icons">clear</i></button>
    </span>
    <span class="filter-button filter-button-right" v-show="code != 'C02'">
      <button @click="setView('days')"><i class="material-icons">view_list</i></button>
      <button @click="setView('week')"><i class="material-icons">view_column</i></button>
    </span>
  </div>
</template>

<script>
export default {
  name: 'DataFilter',
  props: ['code', 'disabled'],
  data(){
    return {
      filter: {
        code : this.code,
        tag: '태그',
        date: null,
        title: null
      },
      tagList: false
    }
  },
  computed: {
    date(){
      return this.$store.state.date.join('.')
    },
    tag(){
      return this.$store.state.tag
    }
  },
  methods: {
    getData(){
      this.$store.dispatch('getData', this.filter)
    },
    clear(){
      this.filter.date = this.filter.title = null
      this.filter.tag = ''

      switch(this.code){
        case 'C01':
          this.$store.dispatch('getRecord', this.date)
          break
        case 'C02':
          this.$store.dispatch('getData', this.filter)
          break
        default: break
      }
    },
    setView(value){
      this.$router.push({name: value})
    }
  },
  mounted(){
    this.$store.dispatch('getTag', this.code)
  }
}
</script>

<style lang="scss" scoped>
@media (min-width: 1281px) {
  /* ##Device = Desktops ##Screen = 1281px to higher resolution desktops */
  .filter {
    display: flex;
    justify-content: stretch;
    align-items: stretch;
    margin-top: 10px;
    padding: 0 10px 10px 10px;
    border-bottom: 1px solid #ccc;

    .filter-select {
      // flex: 1;
      position: relative;

      button {
        display: flex;
        align-items: center;
        position: relative;
        box-sizing: border-box;
        margin: 0;
        padding: 0;
        width: 100%;
        height: 30px;
        border: 1px solid #222;
        border-radius: 3px;
        background-color: #333;
        box-shadow: inset 1px 1px 0 #444;
        font-size: 12px;
        color: #fff;
        cursor: pointer;

        span {
          flex: 1;
          padding-left: 5px;
          text-align: left;
          color: #fff;
        }
      }

      ul {
        position: absolute;
        bottom: 0;
        top: 29px;
        z-index: 1;
        box-sizing: border-box;
        margin: 0;
        padding: 0;
        width: 100%;
        border-bottom-left-radius: 3px;
        border-bottom-right-radius: 3px;

        li {
          margin-top: -1px;
          border: 1px solid #222;
          background-color: #333;
          box-shadow: inset 1px 1px 0 #444;

          &:last-child {
            border-bottom-left-radius: 3px;
            border-bottom-right-radius: 3px;
          }

          button {
            margin: 0;
            padding: 0 0 0 5px;
            width: 100%;
            height: 30px;
            border: 0;
            background-color: transparent;
          }
        }
      }
    }
    .filter-input {
      // flex: 1;
      padding-left: 5px;

      label {
        float: left;
        box-sizing: border-box;
        padding: 0 5px;
        height: 30px;
        border: 1px solid #222;
        border-top-left-radius: 3px;
        border-bottom-left-radius: 3px;
        background-color: #333;
        box-shadow: inset 1px 1px 0 #444;
        font-size: 12px;
        line-height: 30px;
        color: #fff;
      }
      input {
        float: left;
        box-sizing: border-box;
        margin: 0;
        padding: 0 0 0 5px;
        height: 30px;
        border: 1px solid #222;
        border-top-right-radius: 3px;
        border-bottom-right-radius: 3px;
        background-color: #333;
        box-shadow: inset 1px 1px 3px #222;
        font-size: 12px;
        color: #fff;

        &::-webkit-outer-spin-button,
        &::-webkit-inner-spin-button {
          -webkit-appearance: none;
        }
      }
    }

    .filter-button {
      &::after {
        content: '';
        display: block;
        clear: both;
      }

      button {
        float: left;
        display: flex;
        justify-content: center;
        align-items: center;
        box-sizing: border-box;
        margin: 0 0 0 5px;
        padding: 0;
        width: 30px;
        height: 30px;
        border: 1px solid #222;
        border-radius: 3px;
        background-color: #333;
        box-shadow: inset 1px 1px 0 #444;
        color: #fff;
        cursor: pointer;
      }
    }

    .filter-right {
      float: right;
    }
  }
}

@media (min-width: 1025px) and (max-width: 1280px) {
  /* ##Device = Laptops, Desktops ##Screen = B/w 1025px to 1280px */
}

@media (min-width: 768px) and (max-width: 1024px) {
  /* ##Device = Tablets, Ipads (portrait) ##Screen = B/w 768px to 1024px */
}

@media (min-width: 768px) and (max-width: 1024px) and (orientation: landscape) {
  /* ##Device = Tablets, Ipads (landscape) ##Screen = B/w 768px to 1024px */
}

@media (min-width: 481px) and (max-width: 767px) {
  /* ##Device = Low Resolution Tablets, Mobiles (Landscape) ##Screen = B/w 481px to 767px */
}

@media (min-width: 320px) and (max-width: 480px) {
  /* ##Device = Most of the Smartphones Mobiles (Portrait) ##Screen = B/w 320px to 479px */
}
</style>