<template>
  <div class="home">
    <div v-if="projects.length">
      <div v-for="project in projects" :key="project.id">
        <SingleProject @complete="handleComplete" @delete="handleDelete" :project="project"/>
      </div>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src

import SingleProject from "../components/SingleProject.vue"
export default {
  name: 'Home',
  components: {SingleProject},
  data(){
    return{
      projects:[]
    }
  },
  methods:{
    handleDelete(id){
      this.projects=this.projects.filter((project)=>{
        return project.id !== id
      })
    },
    handleComplete(id){
      let p=this.projects.find(project=>{
        return project.id===id
      })
      p.complete=!p.complete
    }
  },
  
  async mounted(){
    try {
      const res=await fetch("http://localhost:3000/projects")
      const data=await res.json()
      this.projects=data
    } catch (error) {
      console.log(error);
    }
   
  }
}
</script>
