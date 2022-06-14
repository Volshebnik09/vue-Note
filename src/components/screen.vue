<template>
	<div class="screen">
	    <div
	      v-if="sections.filter(el => el.id == sectionSelected).length>0"
	      class="wrapper">
	    	<div class="screen__top">
	        	<img 
	        		class="screen__top__icon" 
			        v-bind:src="img[sections.filter(el => el.id == sectionSelected)[0].icon]" 
			        alt="">
			    <div 
			    	class="screen__top__title" 
			    	@keyup.enter="saveNameSection($event,sections.filter(el => 
			    	el.id == sectionSelected)[0].id)" 
			    	contenteditable=true>
			          {{sections.filter(el => el.id == sectionSelected)[0].name}}
	        	</div>
	      	</div>
	      	<div class="screen__section-lines">
	      		<div class="screen__section-line"
	      		v-for="(line, id) in sections.filter(el => el.id == sectionSelected)[0].lines" 
	      		>
		      		<div
		      		class="screen__section-content__line" contenteditable="true"
		      		v-html="line"
		      		@blur = "saveLines()"

		      		></div>
	      			<img 
	      			@click="deleteLine($event)"

	      			class="screen__section-content__line__delete" 
	      			src="@/img/cancel.svg">
	      		</div>
	      	</div>
	      	<div 
	      	class="screen__section-add-new-line"
	      	@click="addNewLine($event)">Нажми,чтобы добавить новую линию.</div>
	    </div>
  	</div>
</template>

<script>
import favicon from "@/img/favicon.ico"

export default {
  props:{
    sections:{},
    sectionSelected:{},
  },
  name: 'screen',

  data () {
    return {
    	img:{
    		favicon:favicon
    	}
    }
  },

  methods: {
  		deleteLine(event){
  			console.log(event.target.parentNode.remove())
  			this.saveLines()
  		},
    	saveNameSection(event, id) {
    		this.$emit('saveNameSection',event,id)
    	},
    	saveLines(addtitional=false){
    		let lines = document.querySelectorAll('.screen__section-content__line')
    		let realLines = []
    		lines.forEach((line)=>realLines.push(line.innerHTML))
    		if (addtitional) realLines.push('')
    		console.log(realLines)
    		this.$emit('saveLines', realLines, this.sectionSelected)
    	},
    	addNewLine(event){
    		this.saveLines(true)
    		// let line = document.createElement('div')
    		// let lineExit = document.createElement('img')
    		// let lineContent = document.createElement('div')
    		// lineExit.src = "cancel.svg"
    		// lineContent.setAttribute('contenteditable','true')
    		// line.className = 'screen__section-line'
    		// lineContent.className = 'screen__section-content__line'
    		// lineExit.className = 'screen__section-content__line__delete' 
    		// lineExit.addEventListener('click',(event)=>{
    		// 	event.target.parentNode.remove()
    		// })
    		// line.appendChild(lineContent)
    		// line.appendChild(lineExit)
    		// document.querySelector('.screen__section-lines').appendChild(line)
    	}
  },
  mounted(){

  }
}
</script>

<style >
	.screen__top__title {
		height: 60px;
    font-family: 'Montserrat';
  font-style: normal;
  font-weight: 600;
  font-size: 54px;

  /* identical to box height */

  color: #FFFFFF;
  }
  [contentEditable=true]:empty:not(:focus):before{
    content:attr(data-text)
}
  .screen__top__title {
    width: 100%;
    line-height: 50px;
    overflow: hidden;
    text-overflow: ellipsis;
  }
  .screen {
    width: 100%;
    height: 100vh;
    background: #202025;
    padding-top: 80px;
  }

  .screen__top {
  	    max-width: 1000px;
    width: 100%;
    margin: auto;
    display: flex;
    align-items: center;
    gap: 10px;
    border-bottom: 1px solid white;
    padding-left: 10px;
    padding-bottom: 10px;
  	margin-bottom: 10px;


  }
  .screen__top br {
    display: none;
  }

  .screen__top__icon {
    height: 50px;
    width: 50px;
  }	
  .screen__section-content__line {
  	position: relative;
  	z-index: 1;
	font-family: 'Montserrat';
	font-style: normal;
	font-weight: 300;
	font-size: 20px;
	line-height: 22px;

	color: #FAFAFA;


  }
  .screen__section-line {
  	position: relative;
  	max-width: 1060px;
  	padding: 0 30px;
    width: 100%;
    margin: auto;
  }
  .screen__section-content__line__delete {
  	position: absolute;
  	height: 20px;
  	top: 0;
  	color: white;
  	left: 0px;
  	cursor: pointer;
  	z-index: 10;
  	transition: 1s;
  	display: block;
  }
  .screen__section-line:hover .screen__section-content__line{
	outline: 1px solid #0060df;  	
  }
  .screen__section-content__line:focus {
  }
  .screen__section-add-new-line{
	font-family: 'Montserrat';
	font-style: normal;
	font-weight: 300;
	font-size: 20px;
	line-height: 22px;

	color: #7A8A8A;  

	cursor: pointer;
	  	position: relative;
  	max-width: 1060px;
  	padding: 0 30px;
    width: 100%;
    margin: auto;
}
</style>