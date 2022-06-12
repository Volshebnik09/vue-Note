<template>
  <div class="menu">
    <div class="btns-menu">
      <button
      @click="addSection"
      >Создать страницу</button>
    </div>
    <div class="sections">
      <div class="section"
      v-for="section in sections"
      :key="section.id">
        <div class="section__content" contenteditable="false" >
          <div class="section__icon">
            <img v-bind:src="section.icon" v-if="section.icon.length>0" alt="">
          </div >
        {{section.name}}
        </div>
        <img src="cogwheel.svg" class="section__cogwheel">
        <img
        @click="removeSection(section.id)"
         src="trashbin.svg" class="section__trashbin">
      </div>
    </div>
  </div> 
</template>

<script>
export default {
  name: 'App',
  data () {
    return {
      sections: [
        {
          id:1,
          icon: 'favicon.ico',
          name: 'example',
        },
        {
          id:2,
          icon: 'favicon.ico',
          name: 'example2',
        },

      ] 
    }
  },
  methods: {
    removeSection(id){
      this.sections = this.sections.filter(el => el.id != id)
    },
    addSection(){
      if (this.sections.length > 0)
        this.sections.push({
          id:this.sections[this.sections.length-1].id + 1,
          icon:'',
          name:'new',
        }) 
      else
        this.sections.push({
        id:0,
        icon:'',
        name:'new',
      })

    }
  },
}
</script>

<style>
  * {
    margin: 0;  
    padding: 0;
    box-sizing: border-box;
  }
  .section__icon{

  }
  .menu {
    max-width: 300px;
    height: 100vh;
    background: #191919;
  }
  .sections {
    display: flex;
    flex-direction: column;
    gap: 5px;
  }
  .section__icon {
    height: 14px;
    width: 14px;
  }
  .section__icon img {
    width: 100%;
    height: 100%;
  }

  .section {
    color: white;
    display: flex;
    align-items: center;
    gap: 10px;
    height: 30px;
    margin-left: 10px;

    font-family: 'Montserrat';
    font-style: normal;
    font-weight: 500;
    font-size: 18px;
    line-height: 22px;

    /* identical to box height */

    color: #FFFFFF;
  }
  .section__content {
    display: flex;
    align-items: center;
    gap: 8px;
    padding: 0 8px;
    width: 155px;
    height: 30px;

  }
  .section:hover .section__content{
    background: #2A2A2A;
  }
</style>