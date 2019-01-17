<template>
  <div class="hello">
    <div class="filter" @click="showDropdown=false">
      <div class="media-container">
        <div class="sizes">
          <div class="less-size" @click="decreaseFontSize">
            А
          </div>
          <div class="more-size" @click="increaseFontSize">
            А
          </div>
        </div>
        <div class="background-selectors">
          <div class="white" @click="whitePreset"></div>
          <div class="beige" @click="beigePreset"></div>
          <div class="gray" @click="grayPreset"></div>
          <div class="black" @click="blackPreset"></div>
        </div>
      </div>
      <div class="settingsContainer">
      <div class="settings" @click="showDropdownMethod">
        Настройки
        <img src="../../public/triangle.png">
      </div>
      <div class="dropdown" v-if="showDropdown" @click="importantShow">
        <label class="font-label">Шрифт</label>
        <select v-model="fontFamily">
          <option>
            Arial 
          </option>
          <option>
            Arial Black
          </option>
          <option>
            Arial Narrow
          </option>
          <option>
            Book Antiqua
          </option>
          <option>
            Century Gothic
          </option>
          <option>
            Comic Sans MS
          </option>
          <option>
            Courier New
          </option>
          <option>
            Franklin Gothic Medium
          </option>
          <option>
            Garamond
          </option>
          <option>
            Georgia
          </option>
          <option>
            Impact
          </option>
          <option>
            Lucida Console
          </option>
          <option>
            Microsoft Sans Serif
          </option>
          <option>
            Palatino Linotype
          </option>
          <option>
            Tahoma
          </option>
          <option>
            Times New Roman
          </option>
          <option>
            Trebuchet Ms
          </option>
          <option>
            Verdana
          </option>
  
        </select>
        <div class="font-size">
          <div class="input-container">
            <label>Размер шрифта</label>
            <input v-model="fontSize" class="font-size_input">
          </div>
          <input type="range" v-model="fontSize" min="9" max="99">
        </div>

        <div class="text-align">
          <div class="left" @click="textAlign='left'">
            <img src="../../public/left.png">
          </div>
          <div class="center" @click="textAlign='center'">
             <img src="../../public/center.png">
          </div>
          <div class="right" @click="textAlign='right'">
             <img src="../../public/right.png">
          </div>
          <div class="justify" @click="textAlign='justify'">
             <img src="../../public/justify.png">
          </div>
        </div>
        <div class="background-color">
          <label>Цвет фона</label>
          <input v-model="backgroundColor" @click="showBackgroundPickerMethod" class="picker_input">
          <color-picker v-model="backgroundColor" class="picker" :height="195" :width="195" v-if="showBackgroundPicker"></color-picker>
        </div>
        <div class="text-color">
          <label>Цвет шрифта</label>
          <input v-model="textColor" @click="showColorPickerMethod" class="picker_input">
          <color-picker v-model="textColor" class="picker" :height="195" :width="195" v-if="showColorPicker"></color-picker>
        </div>
        <div class="clear" @click="clear">
          Очистить
        </div>
      </div>
      </div>
      <div class="save-container">
      <div class="save" @click="saveToStorage">
        Сохранить
      </div>
      <div class="saved" v-if="showSavedLabel">
        Сохранено
      </div>
      </div>
    </div>
    <input v-model="pageNumber" @keyup.enter="locationReplace" class="page-number_input">
    <label @click="locationReplace" class="page-number_label">OK</label>
  </div>
</template>

<script>
import ColorPicker from 'vue-color-picker-wheel';

export default {
  name: 'HelloWorld',
  components: {
                ColorPicker
  },
  data () {
    return {
      fontSize: 18,
      backgroundColor: '#FFFFFF',
      textColor: '#000000',
      textAlign: 'justify',
      showDropdown: false,
      showBackgroundPicker: false,
      showColorPicker: false,
      fontFamily: 'Arial',
      showSavedLabel: false,
      pageNumber: null,
      pageOrigin: null,
      pagePathname: null
    }
  },
  created() {
    const storagedFontSize = +localStorage.getItem('fontSize')
    if (storagedFontSize) this.fontSize = storagedFontSize
    const storagedBackgroundColor = localStorage.getItem('backgroundColor')
    if (storagedBackgroundColor) this.backgroundColor = storagedBackgroundColor
    const storagedTextColor = localStorage.getItem('textColor')
    if (storagedTextColor) this.textColor = storagedTextColor
    const storagedTextAlign = localStorage.getItem('textAlign')
    if (storagedTextAlign) this.textAlign = storagedTextAlign
    const storagedFontFamily = localStorage.getItem('fontFamily')
    if (storagedFontFamily) this.fontFamily = storagedFontFamily
    this.pageNumber = +window.location.search.replace('?page=', '')
    this.pageOrigin = window.location.origin
    this.pagePathname = window.location.pathname
    document.body.addEventListener('click', (e) => {
        this.showDropdown = false
        this.showBackgroundPicker = false
        this.showColorPicker = false
    })
  },

  mounted() {
    document.querySelector('#readerText').style.fontFamily = this.fontFamily;
    document.querySelector('#readerText').style.fontSize = this.fontSize + 'px';
    document.querySelector('#readerText').style.color = this.textColor;
    document.querySelector('#readerText').style.textAlign = this.textAlign;
    document.querySelector('#readerText').style.background = this.backgroundColor;
  },

  watch: {
    backgroundColor: function(newB, oldB) {
        document.querySelector('#readerText').style.background = this.backgroundColor;
    },
    fontSize: function(newB, oldB) {
      document.querySelector('#readerText').style.fontSize = this.fontSize + 'px';
    },
    textColor: function(newB, oldB) {
      document.querySelector('#readerText').style.color = this.textColor;
    },
    textAlign: function(newB, oldB) {
      document.querySelector('#readerText').style.textAlign = this.textAlign;
    },
    fontFamily: function(newB, oldB) {
      document.querySelector('#readerText').style.fontFamily = this.fontFamily;
    }
  },

  methods: {
    clear() {
      this.fontSize = 18
      this.backgroundColor = '#FFFFFF'  
      this.textColor = '#000000'  
      this.textAlign = 'justify'
      this.fontFamily = 'Arial'
      this.saveToStorage()
    },
    locationReplace() {
      const newUrl = this.pageOrigin + this.pagePathname + `?page=${this.pageNumber}`
      window.location.replace(newUrl)
    },
    whitePreset() {
      this.backgroundColor='#FFFFFF'
      this.textColor = '#000000' 
    },
    beigePreset() {
      this.backgroundColor='#f8f1e3'
      this.textColor = '#000000' 
    }, 
    grayPreset() {
      this.backgroundColor='#5a5a5a'
      this.textColor = '#ffffff'
    },
    blackPreset() {
      this.backgroundColor='#121212'
      this.textColor = '#b0b0b0'
    },
    importantShow(event) {
      event.stopPropagation()
      this.showDropdown = true
    },
    saveToStorage() {
      localStorage.setItem('fontSize', this.fontSize)
      localStorage.setItem('backgroundColor', this.backgroundColor)
      localStorage.setItem('textColor', this.textColor)
      localStorage.setItem('textAlign', this.textAlign)
      localStorage.setItem('fontFamily', this.fontFamily)
      this.showSavedLabel = true
      setTimeout(() => this.showSavedLabel = false, 3000)
    },
    showDropdownMethod(event) {
      event.stopPropagation()
      this.showDropdown = !this.showDropdown
    },
    showBackgroundPickerMethod(event) {
      event.stopPropagation()
      this.showColorPicker = false
      this.showBackgroundPicker = true
    },
    showColorPickerMethod(event) {
      event.stopPropagation()
      this.showBackgroundPicker = false
      this.showColorPicker = true
    },
    decreaseFontSize() {
      if (this.fontSize >= 9) {
        this.fontSize -= 1
      }
    },
    increaseFontSize() {
      if (+this.fontSize < 99) {
      this.fontSize = +this.fontSize + 1
      }
    }
  }

}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

.hello {
  max-width: 1000px;
  margin: 0 auto;
}

body {
  transition: all 0.2s linear;
}
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
.filter {
  display: flex;
  align-items: center;
  font-family: 'Roboto', sans-serif;
  justify-content: flex-end;
  margin-bottom: 30px;
  background-color: #fff;
  border-radius: 4px;
  padding: 20px;
}
.less-size {
  padding: 5px;
  width: 18px;
  height: 18px;
  border: 1px solid #000;
  border-radius: 3px;
  font-size: 12px;
  display: flex;
  align-items: center;
  justify-content: center;
  margin-right: 3px;
}
.more-size {
  font-size: 18px;
  padding: 0px;
  width: 18px;
  height: 18px;
  padding: 5px;
  border: 1px solid #000;
  border-radius: 3px;
  display: flex;
  align-items: center;
  justify-content: center;
}
.background-selectors {
  margin-left: 6px;
  display: flex;
}
.white, .beige, .gray, .black, .right, .left, .center, .justify {
  width: 18px;
  height: 18px;
  padding: 5px;
  border: 1px solid #000;
  border-radius: 3px;
  margin-right: 3px;
  cursor: pointer;
}
.white {
  background: white;
}
.beige {
  background: #f8f1e3;
}
.gray {
  background: #5a5a5a;
}
.black {
  background: #121212;
}
label {
  text-align: left;
}
.dropdown {
  min-width: 160px;
  max-width: 360px;
  list-style: none;
  background: #fff;
  border: solid 1px #ddd;
  border-radius: 4px;
  box-shadow: 0 5px 10px rgba(0,0,0,.2);
  overflow: visible;
  padding: 20px;
  display: flex;
  flex-direction: column;
  max-width: 261px;
  align-items: flex-start;
  margin: 0;
  position: absolute;
  z-index: 2;
}

.dropdown>div {
  margin-top: 15px;
  width: 100%;
}

input[type=range] {
  margin-top: 15px;
  -webkit-appearance: none; /* Hides the slider so that custom slider can be made */
  width: 100%; /* Specific width is required for Firefox. */
  background: transparent; /* Otherwise white in Chrome */
}

input[type=range]::-webkit-slider-thumb {
  -webkit-appearance: none;
}

input[type=range]:focus {
  outline: none; /* Removes the blue border. You should probably do some kind of focus styling for accessibility reasons though. */
}

input[type=range]::-ms-track {
  width: 100%;
  cursor: pointer;

  /* Hides the slider so custom styles can be added */
  background: transparent; 
  border-color: transparent;
  color: transparent;
}
input[type=range]::-webkit-slider-runnable-track {
  width: 100%;
  height: 8.4px;
  cursor: pointer;
  box-shadow: 1px 1px 1px #000000, 0px 0px 1px #0d0d0d;
  background: #db6974;
  border-radius: 1.3px;
  border: 0.2px solid #010101;
}

input[type=range]:focus::-webkit-slider-runnable-track {
  background: #db6974;
}

input[type=range]::-moz-range-track {
  width: 100%;
  height: 8.4px;
  cursor: pointer;
  box-shadow: 1px 1px 1px #000000, 0px 0px 1px #0d0d0d;
  background: #3071a9;
  border-radius: 1.3px;
  border: 0.2px solid #010101;
}

input[type=range]::-ms-track {
  width: 100%;
  height: 8.4px;
  cursor: pointer;
  background: transparent;
  border-color: transparent;
  border-width: 16px 0;
  color: transparent;
}
input[type=range]::-ms-fill-lower {
  background: #db6974;
  border: 0.2px solid #010101;
  border-radius: 2.6px;
  box-shadow: 1px 1px 1px #000000, 0px 0px 1px #0d0d0d;
}
input[type=range]:focus::-ms-fill-lower {
  background: #db6974;
}
input[type=range]::-ms-fill-upper {
  background: #db6974;
  border: 0.2px solid #010101;
  border-radius: 2.6px;
  box-shadow: 1px 1px 1px #000000, 0px 0px 1px #0d0d0d;
}
input[type=range]:focus::-ms-fill-upper {
  background: #db6974;
}

input[type=range]::-webkit-slider-thumb {
  -webkit-appearance: none;
  height: 30px;
  width: 30px;
  border: none;
  border-radius: 50px;
  background:white url('../../public/slider.png');
  cursor: pointer;
  margin-top: -12px; /* You need to specify a margin in Chrome, but in Firefox and IE it is automatic */
}


/* All the same stuff for Firefox */
input[type=range]::-moz-range-thumb {
  height: 30px;
  width: 30px;
  background: #ffffff;
  cursor: pointer;
}

.text-align {
  display: flex;
  width: 120px !important;
}

.text-align div {
  cursor: pointer;
  font-size: 10px;
}

.picker {
  cursor: crosshair;
  width: 195px;
  height: 195px;
}

.text-color, .background-color {
  display: flex;
  flex-direction: column;
}
.text-color label, .background-color label {
  cursor: pointer
}

.text-color:hover,
.background-color:hover,
.text-color:hover .picker_input,
.background-color:hover .picker_input {
  color: #db6974;
}

.font-size {
  display: flex;
  flex-direction: column;
  align-items: flex-start;
}

.settings {
  color: #db6974;
  background-color: #fff;
  font-size: 1.4rem;
  border-bottom: 1px solid #db6974;
  cursor: pointer;
}

.save {
  font-family: 'Roboto', sans-serif;
  font-size: 1.4rem;
  font-weight: bold;
  border-bottom: 1px solid #db6974;
  color: #db6974;
  cursor: pointer;
}

select {
      width: 212px;
    height: 34px;
    border-top-left-radius: 4px;
    border-top-right-radius: 4px;
    border: 1px solid #ccc;
        margin-top: 9px;
}

.font-size_input, .page-number_input {
      border: 1px solid #999;
    padding: 3px 10px;
    margin: 5px 0;
    border-radius: 10px;
    background: transparent none repeat scroll 0 0;
    font-size: 1.4rem;
    background-color: #fff;
    max-width: 59px;
}

.picker_input {
    font-size: 1.1rem;
    border: 1px solid #999;
    padding: 3px 10px;
    margin: 5px 0;
    border-radius: 10px;
    background: transparent none repeat scroll 0 0;
    background-color: #fff;
    max-width: 70px;
}

.input-container {
  display: flex;
  align-items: center;
}

label {
  font-size: 1.2rem;
  margin-right: 5px;
}

.clear {
      font-size: 1.4rem;
    color: #333;
    border-bottom: 1px solid #797979;
    width: 62.45px !important;
    cursor: pointer;
}

.sizes {
  display: flex;
}

.sizes>div {
  cursor: pointer;
  color: black !important;
}

.sizes>div:hover {
  color: #db6974;
}

.filter>div {
  margin-left: 20px;
}

.saved {
  font-weight: bold;
  font-size: 1.3rem;
  color: #1eae00;
  margin-top: 6px;
  position: absolute;
  z-index: 1;
}

.save-container {
  position: relative;
}

.media-container {
  display: flex;
  align-items: center;
}

@media (max-width: 600px) {
  .filter {
    flex-direction: column-reverse;
    align-items: flex-end;
  }

  .filter>div {
    margin-bottom: 30px;
  }
}

@media (max-width: 1200px) {
  .dropdown {
        right: 8px;
  }
}

.page-number_input {
  font-size: 1.8rem !important;
  text-align: center;
}

.page-number_label {
  font-size: 1.8rem;
  padding-left: 7px;
  cursor: pointer;
}
</style>
