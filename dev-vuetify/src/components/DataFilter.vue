<template>
  <div class="filter">
    <div class="filter-left">
      <span class="filter-select" v-show="!disabled">
        <button @click="tagList = !tagList"><span>{{ filter.tag == '' ? '전체' : filter.tag }}</span><i class="material-icons">keyboard_arrow_down</i></button>
        <ul v-show="tagList">
          <li><button @click="filter.tag = ''; tagList = false">전체</button></li>
          <li
            v-for="item in tag"
            v-bind:key="item.key">
            <button @click="filter.tag = item; tagList = false">{{ item }}</button>
          </li>
        </ul>
      </span>
      <span class="filter-input filter-date" v-show="!disabled">
        <label for="FILTER_DATE">날짜</label>
        <input type="text" id="FILTER_DATE" v-model="filter.date" @keyup.enter="getData" @keyup="getNumber" placeholder="YYYYMMDD">
      </span>
      <span class="filter-input" v-show="!disabled">
        <label for="FILTER_TITLE">제목</label>
        <input type="text" id="FILTER_TITLE" v-model="filter.title" @keyup.enter="getData" placeholder="제목 입력">
      </span>
      <span class="filter-button" v-show="!disabled">
        <button @click="getData"><i class="material-icons">search</i></button>
        <button @click="clear"><i class="material-icons">clear</i></button>
      </span>
    </div>
    <div class="filter-right">
      <span class="filter-button" v-show="code != 'C02'">
        <button @click="setView('days')"><i class="material-icons">view_list</i></button>
        <button @click="setView('week')"><i class="material-icons">view_column</i></button>
      </span>
    </div>
  </div>
</template>

<script>
export default {
  name: 'DataFilter',
  props: ['code', 'disabled'],
  data(){
    return {
      filter: {
        code: this.code,
        tag: '',
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
    getNumber(){
      this.filter.date = this.filter.date.replace(/\D/g, '')
    },
    getData(){
      this.$store.dispatch('getData', this.filter)
    },
    clear(){
      this.filter.date = this.filter.title = null
      this.filter.tag = '태그'

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
.filter {
  display: flex;
  flex-flow: wrap;
  justify-content: stretch;
  align-items: stretch;
  margin-top: 10px;
  padding: 0 10px 10px 10px;
  border-bottom: 1px solid #ccc;

  .filter-left {
    flex: 1;
    display: flex;
  }

  .filter-right {
    display: flex;
  }

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
      border: 1px solid #333;
      border-radius: 3px;
      background-color: #444;
      box-shadow: inset 1px 1px 0 #555;
      font-size: 12px;
      color: #fff;
      cursor: pointer;

      span {
        flex: 1;
        padding-left: 5px;
        text-align: left;
        color: #fff;
        white-space: nowrap;
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
        border: 1px solid #333;
        background-color: #444;
        box-shadow: inset 1px 1px 0 #555;

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
    display: flex;
    padding-left: 5px;
    &::after {
      content: '';
      display: block;
      clear: both;
    }

    label {
      box-sizing: border-box;
      padding: 0 5px;
      height: 30px;
      border: 1px solid #333;
      border-top-left-radius: 3px;
      border-bottom-left-radius: 3px;
      background-color: #444;
      box-shadow: inset 1px 1px 0 #555;
      font-size: 12px;
      line-height: 28px;
      color: #fff;
      white-space: nowrap;
      cursor: pointer;
    }

    input {
      flex: 1;
      box-sizing: border-box;
      margin: 0 0 0 -1px;
      padding: 0 0 0 5px;
      height: 30px;
      border: 1px solid #333;
      border-top-right-radius: 3px;
      border-bottom-right-radius: 3px;
      background-color: #444;
      box-shadow: inset 0px 1px 3px #333;
      font-size: 12px;
      color: #fff;
      outline: none;
    }
  }

  .filter-date input {
    width: 84px;
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
      border: 1px solid #333;
      border-radius: 3px;
      background-color: #444;
      box-shadow: inset 1px 1px 0 #555;
      color: #fff;
      cursor: pointer;
    }
  }

  .filter-button-right {
    float: right;
  }
}

@media (min-width: 1281px) {
  /* ##Device = Desktops ##Screen = 1281px to higher resolution desktops */
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