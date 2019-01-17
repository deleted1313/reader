<template>
  <div class="readerMax">
    <div class="readerMax-filter" @click="showDropdown=false">
      <div class="readerMax-media-container">
        <div class="readerMax-sizes">
          <div class="readerMax-less-size" @click="decreaseFontSize">
            А
          </div>
          <div class="readerMax-more-size" @click="increaseFontSize">
            А
          </div>
        </div>
        <div class="readerMax-background-selectors">
          <div class="readerMax-white" @click="whitePreset"></div>
          <div class="readerMax-beige" @click="beigePreset"></div>
          <div class="readerMax-gray" @click="grayPreset"></div>
          <div class="readerMax-black" @click="blackPreset"></div>
        </div>
      </div>
      <div class="readerMax-settingsContainer">
      <div class="readerMax-settings" @click="showDropdownMethod">
        Настройки
        <img src="../../public/triangle.png">
      </div>
      <div class="readerMax-dropdown" v-if="showDropdown" @click="importantShow">
        <label class="readerMax-font-label">Шрифт</label>
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
        <div class="readerMax-font-size">
          <div class="readerMax-input-container">
            <label>Размер шрифта</label>
            <input v-model="fontSize" class="readerMax-font-size_input">
          </div>
          <input class="readerMax-range" type="range" v-model="fontSize" min="9" max="99">
        </div>

        <div class="readerMax-text-align">
          <div class="readerMax-left" @click="textAlign='left'">
            <img src="../../public/left.png">
          </div>
          <div class="readerMax-center" @click="textAlign='center'">
             <img src="../../public/center.png">
          </div>
          <div class="readerMax-right" @click="textAlign='right'">
             <img src="../../public/right.png">
          </div>
          <div class="readerMax-justify" @click="textAlign='justify'">
             <img src="../../public/justify.png">
          </div>
        </div>
        <div class="readerMax-background-color">
          <label>Цвет фона</label>
          <input v-model="backgroundColor" @click="showBackgroundPickerMethod" class="readerMax-picker_input">
          <color-picker v-model="backgroundColor" class="readerMax-picker" :height="195" :width="195" v-if="showBackgroundPicker"></color-picker>
        </div>
        <div class="readerMax-text-color">
          <label>Цвет шрифта</label>
          <input v-model="textColor" @click="showColorPickerMethod" class="readerMax-picker_input">
          <color-picker v-model="textColor" class="readerMax-picker" :height="195" :width="195" v-if="showColorPicker"></color-picker>
        </div>
        <div class="readerMax-clear" @click="clear">
          Очистить
        </div>
      </div>
      </div>
      <div class="readerMax-save-container">
      <div class="readerMax-save" @click="saveToStorage">
        Сохранить
      </div>
      <div class="readerMax-saved" v-if="showSavedLabel">
        Сохранено
      </div>
      </div>
    </div>
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
    document.querySelector('body').style.background = this.backgroundColor;
  },

  watch: {
    backgroundColor: function(newB, oldB) {
        document.querySelector('body').style.background = this.backgroundColor;
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
<style>
.readerMax-filter {
  display: flex;
  align-items: center;
  font-family: 'Roboto', sans-serif;
  justify-content: flex-end;
  margin-bottom: 30px;
  background-color: #fff;
  border-radius: 4px;
  padding: 20px;
}
.readerMax-less-size {
  padding: 5px;
      width: 28px;
    height: 28px;
  border: 1px solid #000;
  border-radius: 3px;
  font-size: 12px;
  display: flex;
  align-items: center;
  justify-content: center;
  margin-right: 3px;
}
.readerMax-more-size {
  font-size: 18px;
  padding: 0px;
      width: 28px;
    height: 28px;
  padding: 5px;
  border: 1px solid #000;
  border-radius: 3px;
  display: flex;
  align-items: center;
  justify-content: center;
}
.readerMax-background-selectors {
  margin-left: 6px;
  display: flex;
}
.readerMax-white, .readerMax-beige, .readerMax-gray, .readerMax-black, .readerMax-right, .readerMax-left, .readerMax-center, .readerMax-justify {
      width: 28px;
    height: 28px;
  padding: 5px;
  border: 1px solid #000;
  border-radius: 3px;
  margin-right: 3px;
  cursor: pointer;
}
.readerMax-white {
  background: white;
}
.readerMax-beige {
  background: #f8f1e3;
}
.readerMax-gray {
  background: #5a5a5a;
}
.readerMax-black {
  background: #121212;
}

.readerMax-dropdown {
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

.readerMax-dropdown>div {
  margin-top: 15px;
  width: 100%;
}

.readerMax input[type=range] {
  margin-top: 15px;
  -webkit-appearance: none; /* Hides the slider so that custom slider can be made */
  width: 100%; /* Specific width is required for Firefox. */
  background: transparent; /* Otherwise white in Chrome */
}

.readerMax input[type=range]::-webkit-slider-thumb {
  -webkit-appearance: none;
}

.readerMax input[type=range]:focus {
  outline: none; /* Removes the blue border. You should probably do some kind of focus styling for accessibility reasons though. */
}

.readerMax input[type=range]::-ms-track {
  width: 100%;
  cursor: pointer;

  /* Hides the slider so custom styles can be added */
  background: transparent; 
  border-color: transparent;
  color: transparent;
}
.readerMax input[type=range]::-webkit-slider-runnable-track {
  width: 100%;
  height: 8.4px;
  cursor: pointer;
  box-shadow: 1px 1px 1px #000000, 0px 0px 1px #0d0d0d;
  background: #1976d2;
  border-radius: 1.3px;
  border: 0.2px solid #010101;
}

.readerMax input[type=range]:focus::-webkit-slider-runnable-track {
  background: #1976d2;
}

.readerMax input[type=range]::-moz-range-track {
  width: 100%;
  height: 8.4px;
  cursor: pointer;
  box-shadow: 1px 1px 1px #000000, 0px 0px 1px #0d0d0d;
  background: #3071a9;
  border-radius: 1.3px;
  border: 0.2px solid #010101;
}

.readerMax input[type=range]::-ms-track {
  width: 100%;
  height: 8.4px;
  cursor: pointer;
  background: transparent;
  border-color: transparent;
  border-width: 16px 0;
  color: transparent;
}
.readerMax input[type=range]::-ms-fill-lower {
  background: #1976d2;
  border: 0.2px solid #010101;
  border-radius: 2.6px;
  box-shadow: 1px 1px 1px #000000, 0px 0px 1px #0d0d0d;
}
.readerMax input[type=range]:focus::-ms-fill-lower {
  background: #1976d2;
}
.readerMax input[type=range]::-ms-fill-upper {
  background: #1976d2;
  border: 0.2px solid #010101;
  border-radius: 2.6px;
  box-shadow: 1px 1px 1px #000000, 0px 0px 1px #0d0d0d;
}
.readerMax input[type=range]:focus::-ms-fill-upper {
  background: #1976d2;
}

.readerMax input[type=range]::-webkit-slider-thumb {
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
.readerMax input[type=range]::-moz-range-thumb {
  height: 30px;
  width: 30px;
  background: #ffffff;
  cursor: pointer;
}

.readerMax-text-align {
  display: flex;
  width: 120px !important;
}

.readerMax-text-align div {
  cursor: pointer;
  font-size: 10px;
}

.readerMax-picker {
  cursor: crosshair;
  width: 195px;
  height: 195px;
}

.readerMax-text-color, .readerMax-background-color {
  display: flex;
  flex-direction: column;
}
.readerMax-text-color label, .readerMax-background-color label {
  cursor: pointer
}

.readerMax-text-color:hover,
.readerMax-background-color:hover,
.readerMax-text-color:hover .readerMax-picker_input,
.readerMax-background-color:hover .readerMax-picker_input {
  color: #1976d2;
}

.readerMax-font-size {
  display: flex;
  flex-direction: column;
  align-items: flex-start;
}

.readerMax-settings {
  color: #1976d2;
  background-color: #fff;
  font-size: 1.4rem;
  border-bottom: 1px solid #1976d2;
  cursor: pointer;
}

.readerMax-save {
  font-family: 'Roboto', sans-serif;
  font-size: 1.4rem;
  font-weight: bold;
  border-bottom: 1px solid #1976d2;
  color: #1976d2;
  cursor: pointer;
}

.readerMax select {
      width: 212px;
    height: 34px;
    border-top-left-radius: 4px;
    border-top-right-radius: 4px;
    border: 1px solid #ccc;
        margin-top: 9px;
}

.readerMax-font-size_input, .readerMax-page-number_input {
      border: 1px solid #999;
    padding: 3px 10px;
    margin: 5px 0;
    border-radius: 10px;
    background: transparent none repeat scroll 0 0;
    font-size: 1.4rem;
    background-color: #fff;
    max-width: 59px;
}

.readerMax-picker_input {
    font-size: 1.1rem;
    border: 1px solid #999;
    padding: 3px 10px;
    margin: 5px 0;
    border-radius: 10px;
    background: transparent none repeat scroll 0 0;
    background-color: #fff;
    max-width: 70px;
}

.readerMax-input-container {
  display: flex;
  align-items: center;
}

.readerMax label {
  font-size: 1.2rem;
  margin-right: 5px;
  text-align: left;
  margin-bottom: 0 !important; 
}

.readerMax-clear {
      font-size: 1.4rem;
    color: #333;
    border-bottom: 1px solid #797979;
    width: 62.45px !important;
    cursor: pointer;
}

.readerMax-sizes {
  display: flex;
}

.readerMax-sizes>div {
  cursor: pointer;
  color: black !important;
}

.readerMax-sizes>div:hover {
  color: #1976d2;
}

.readerMax-filter>div {
  margin-left: 20px;
}

.readerMax-saved {
  font-weight: bold;
  font-size: 1.3rem;
  color: #1eae00;
  margin-top: 6px;
  position: absolute;
  z-index: 1;
}

.readerMax-save-container {
  position: relative;
}

.readerMax-media-container {
  display: flex;
  align-items: center;
}

@media (max-width: 600px) {
  .readerMax-filter {
    flex-direction: column-reverse;
    align-items: flex-end;
  }

  .readerMax-filter>div {
    margin-bottom: 30px;
  }
}

@media (max-width: 1200px) {
  .readerMax-dropdown {
        right: 8px;
  }
}

.readerMax-page-number_input {
  font-size: 1.8rem !important;
  text-align: center;
}

.readerMax-page-number_label {
  font-size: 1.8rem;
  padding-left: 7px;
  cursor: pointer;
}
</style>
