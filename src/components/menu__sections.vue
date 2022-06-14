<template>
	<div class="menu__sections">
      <div class="section"
      v-for="section in sections"
      :key="section.id">
        <div
        @click="selectSection(section.id)"
        :active="section.id == sectionSelected"
        class="section__content"  >
          <div class="section__icon">
            <img  
        	draggable="false" ondragstart="return false;"
            :src="img[section.icon]" 
            v-if="section.icon.length>0" alt="">
          </div>
          <div 
	          class="section__name" 
	          @keyup.enter="saveNameSection($event,section.id)"
	          :contenteditable="section.contenteditable"
          >{{section.name}}</div>
        
        </div>
        <!-- Впринципе можно и отключить cogwheel -->
        <img hidden 
        	draggable="false" ondragstart="return false;"
        	v-if="!section.contenteditable"
        	@click="sectionEditAllow(section.id)"
        	:src="img['cogwheel']" class="section__cogwheel">
        <img
        	draggable="false" ondragstart="return false;"
        	v-if="!section.contenteditable"
	        @click="removeSection(section.id)"
	         :src="img['trashbin']" class="section__trashbin">
        <img
        	draggable="false" ondragstart="return false;"
        	v-if="section.contenteditable"
        	@click="applySectionChanges($event, section.id)"
        	:src="img['ok']" class="section__ok">
        <img
        	draggable="false" ondragstart="return false;"
        	v-if="section.contenteditable"
	        @click="denySectionChanges($event, section.id)"
	        :src="img['cancel']" class="section__cancel">
      </div>
    </div>
</template>

<script>
import cogwheel from "@/img/cogwheel.svg"
import trashbin from "@/img/trashbin.svg"
import ok from "@/img/ok.svg"
import cancel from "@/img/ok.svg"
import favicon from "@/img/favicon.ico"

export default {
  
  name: 'menu__sections',
  props: {
  	sections: {},
  	sectionSelected:{}
  },
  data () {
    return {
      img:{
    cogwheel:cogwheel,
    trashbin:trashbin,
    ok:ok,
    cancel:cancel,
    favicon:favicon
  },
    }
  },
  methods: {
  	selectSection(id){
  		this.$emit('selectSection',id)
  	},
  	sectionEditAllow(id){
  		this.$emit('sectionEditAllow',id)
  	},
    removeSection(id){
      this.$emit('removeSection', id)
      // this.sections = this.sections.filter(el => el.id != id)
    },
    saveNameSection(event,id){
      this.$emit('saveNameSection',event,id)
    },

    addSection(){
     this.$emit('addSection')
    },

    applySectionChanges(event, id){
    	this.$emit('applySectionChanges',event, id)
    },

    denySectionChanges(event, id){
    	this.$emit('denySectionChanges',event, id)
    }
  },
}
</script>

<style lang="css" scoped>
	.menu__sections {

    display: flex;
    flex-direction: column;
    gap: 5px;
  }
  .section__icon {
    height: 30px;
  }
  .section__icon img {
    width: 30px;
    height: 30px;
  }
  .section__name {
    height: 30px;
    outline: none;
    line-height: 30px;
    width: 100%;
    overflow: hidden;
    text-overflow: ellipsis;
  } 

  .section {
    color: white;
    display: flex;
    align-items: center;
    gap: 10px;
    height: 30px;
    margin-left: 10px;
    margin-right: 10px;

    font-family: 'Montserrat';
    font-style: normal;
    font-weight: 500;
    font-size: 18px;
    line-height: 22px;

    /* identical to box height */

    color: #FFFFFF;
  }
  .section:nth-last-child(1){
  }
  .section__content {
  	cursor: pointer;
    display: flex;
    align-items: center;
    gap: 8px;
    width: 155px;
    height: 30px;
  	transition: .1s;

  }
  .section__name[contenteditable="true"] {
  	border: 1px solid white;
  	line-height: 29px;
  }
  .section__content[active="true"]{
  	margin-left: 20px;
    background: #5A5A4F;
  }
  .section:hover .section__content{
    background: #2A2A2A;
  }
  .section__cogwheel {
  	cursor: pointer;
  }
  .section__trashbin{
  	cursor: pointer;
  }
</style>