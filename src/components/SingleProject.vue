<template>
  <div class="project" :class="{complete:project.complete}">
      <div class="actions">
          <h3 @click="toggleDetail">{{project.title}}</h3>
          <div class="icons">
              <span class="material-icons">edit</span>
              <span @click="deleteProject" class="material-icons">delete</span>
              <span @click="toggleComplete"  class="material-icons" :class="{completed:project.complete}">done</span>
          </div>
      </div>
      <div v-show="showDetail" class="details">
          <p>{{project.details}}</p>
          <h1>hello world</h1>
          <p>hello world2</p>
      </div>
  </div>
</template>

<script>
export default {
    props:["project"],
    data(){
        return{
            showDetail:false,
            uri:`http://localhost:3000/projects/${this.project.id}`
           
        }
    },
    mounted(){
        console.log("hello world");
    },
    methods:{
        toggleDetail(){
            this.showDetail=!this.showDetail
        },
        async deleteProject(){
            try {
                await fetch(this.uri,{method:'DELETE'})
                await this.$emit('delete',this.project.id)
            } catch (error) {
                console.log(error);
            }
            
        },
       async toggleComplete (){
           try {
               await fetch(this.uri,{
               method:'PATCH',
               headers:{'Content-Type':'application/json'},
               body:JSON.stringify({complete:!this.project.complete})
               
            })
            await this.$emit('complete',this.project.id)
           } catch (error) {
               console.log(error);
           }
          
        }
    }
}
</script>

<style>
    .project{
        margin: 20px auto;
        background: white;
        padding: 10px 20px;
        border-radius: 4px;
        box-shadow: 1px 2px 3px rgba(0, 0, 0, 0.05);
        border-left: 4px solid #e90074;
    }
    h3{
        cursor: pointer;
    }
    .actions{
        display: flex;
        justify-content: space-between;
        align-items: center;
    }
    .material-icons{
        font-size: 24px;
        margin-left: 10px;
        color: #bbb;
        cursor: pointer;
    }
    .material-icons:hover{
        color: #00ce89;
    }
    .project.complete{
        border-left: 4px solid #00ce89;
    }
    .completed{
        color:#00ce89 ;
    }
</style>