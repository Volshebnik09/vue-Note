<template>
  <div class="menu">
    <div class="top">
      <menu__sections 
        :sections="sections"
        :sectionSelected="sectionSelected"
        @selectSection="selectSection"
        @removeSection="removeSection"
        @saveNameSection="saveNameSection"
        @addSection="addSection"
        @sectionEditAllow="sectionEditAllow"
        @applySectionChanges="applySectionChanges"
        @denySectionChanges="denySectionChanges"
      />
      <button class="btn-add-page"
        @click="addSection"
        >+
      </button>
      
    </div>
    <div class="menu__contacts">
      <a href="https://github.com/Volshebnik09">
        <img :src="img['github']" alt="">
        GitHub
      </a>
      <a href="https://github.com/Volshebnik09/vue-Note">
        <img :src="img['github']" alt="">
        Repository
      </a>
      <a href="https://github.com/Volshebnik09">
        <img :src="img['github']" alt="">
        Main site
      </a>
    </div>
  </div> 
  <screen
  :sections="sections"
  :sectionSelected="sectionSelected"
  @saveNameSection="saveNameSection"
  @saveLines="saveLines"

  />
</template>

<script>
var shiftDown = false;
var setShiftDown = function(event){
    if(event.keyCode === 16 || event.charCode === 16){
        window.shiftDown = true;
    }
};

var setShiftUp = function(event){
    if(event.keyCode === 16 || event.charCode === 16){
        window.shiftDown = false;
    }
};

window.addEventListener? document.addEventListener('keydown', setShiftDown) : document.attachEvent('keydown', setShiftDown);
window.addEventListener? document.addEventListener('keyup', setShiftUp) : document.attachEvent('keyup', setShiftUp);
import menu__sections from "@/components/menu__sections.vue"
import screen from "@/components/screen.vue"
import github from "@/img/github.svg"


export default {
  name: 'App',
  components: {
    menu__sections,screen
  },

  data () {
    return {
      img: {
        github:github
      },
      sections: [
        // {
        //   id:1,
        //   icon: 'favicon.ico',
        //   name: 'example',
        //   contenteditable: false,
        // },
      ],
      sectionSelected: 1
    }
  },

  methods: {

    removeSection(id){
      this.sections = this.sections.filter(el => el.id != id)
      localStorage.sections = JSON.stringify(this.sections)

    },

    saveNameSection(event = 0, id, from = ''){
      if (!window.shiftDown) {  
        if (from) {

          if (from.textContent.length > 0)
            this.sections.map((el)=>{if (el.id === id) el.name = from.textContent})
          else
            this.sections.map((el)=>{if (el.id === id) el.name = "empty name"})
        console.log(this.sections)
          return
        } 
        if (event.target.textContent.length > 0)
          this.sections.map((el)=>{if (el.id === id) el.name = event.target.textContent})
        else
          this.sections.map((el)=>{if (el.id === id) el.name = "empty name"})
        event.target.blur()
        this.sections.map((el)=>{if (el.id === id) el.contenteditable = false } )
        console.log(this.sections)
        localStorage.sections = JSON.stringify(this.sections)
      }
    },

    addSection(){
      if (this.sections.length > 0)
        this.sections.push({
          id:this.sections[this.sections.length-1].id + 1,
          icon:'favicon',
          name:'new section',
        }) 
      else
        this.sections.push({
        id:0,
        icon:'favicon',
        name:'new section',
      })
      setTimeout(()=>{
        document.querySelector('.menu > .top').scrollTop= document.querySelector('.menu > .top').scrollHeight
      },4)
      localStorage.sections = JSON.stringify(this.sections)
    },

    sectionEditAllow(id){
      this.sections.map((el)=>{if (el.id === id) el.prevName = el.name})
      this.sections.map((el)=>{(el.id === id)? el.contenteditable = true: el.contenteditable = false } )
    },

    applySectionChanges(event, id){

      this.saveNameSection(event,id,event.target.parentNode.childNodes[0].childNodes[1])
      this.sections.map((el)=>{if (el.id === id) el.contenteditable = false} )
    },

    denySectionChanges(event, id){

      this.sections.map((el)=>{if (el.id === id) {
        el.name = el.prevName
        delete el.prevName;
        event.target.parentNode.childNodes[0].childNodes[1].textContent= el.name
      }})


      this.sections.map((el)=>{if (el.id === id) el.contenteditable = false} )
      console.log(this.sections)
    },
    selectSection(id){
      this.sectionSelected = id
    },
    saveLines(lines, id){
       this.sections.map((el)=>{
        if (el.id === id) 
          el.lines = lines
       })
        localStorage.sections = JSON.stringify(this.sections)

    }
  },

  mounted() {
    if (localStorage.sections){
      this.sections = JSON.parse(localStorage.sections)
      if (this.sections.length < 1) {
        this.sections.push({
          id:1,
          icon: 'favicon',
          name: 'Example page',
          lines: [
          'Ты можешь писать здесь что угодно',
          'Чтобы добавить новую секцию нажми на + в боковом меню',
          'Сохранение происходит когда ты выходишь из редактируемого поля',
          '<br> В будущем можно будет: <br> 1) Создавать разделитель <br> 2) Использовать различные стили для блоков текста <br> 3) Изменять иконки секций <br> 4) Адаптивность ','<br> Ты можешь удалить эту страницу, чтобы увидеть её снова удали все существующие страницы'],
          contenteditable: false,
        })
      }
    }
    // if (localStorage.sectionSelected){
    //   this.sectionSelected = localStorage.sectionSelected
    // } else {
    //   this.sectionSelected = this.sections[0].id
    //   console.log(this.sectionSelected)
    // }
  },
}
</script>

<style>
  html {
    overflow: hidden;
  }
  #app {
    display: flex;
  }
  
  img {
    user-select: none;
  }
  * {
    margin: 0;  
    padding: 0;
    box-sizing: border-box;
  }
  .menu > .top {
    margin-top: 10px;
    overflow-y: auto;
    scroll-behavior: smooth;

  }
  .menu {
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    width: 300px;
    height: 100vh;
    background: #191919;
  }
  .btn-add-page {
    cursor: pointer;
    display: block;
    background: white;
    outline: none;
    border: none;
    margin: 10px 0 0 10px;
    width: 30px;
    height: 30px;
    line-height: 0px;
    font-size: 30px;
    border-bottom-right-radius: 20px;
    border-bottom-left-radius: 20px;
    transition: 0.1s;
    margin-bottom: 20px;
  }
  .btn-add-page:hover {
    transform: scale(0.96);
  }
  .menu__contacts{
    border-top: 2px solid white;
    margin: 10px;
    padding-top: 10px;
    flex-direction: column;
    gap: 10px;
    display: flex;
  }
  .menu__contacts a {
        font-family: 'Montserrat';
    font-style: normal;
    font-weight: 500;
    font-size: 18px;
    line-height: 22px;
    width: fit-content;
    /* identical to box height */

    color: #FFFFFF;
    display: flex;
    align-items: center;
    gap: 20px;
    text-decoration: none;
  }
</style>