<template>
    <section>
       <coach-filter @change-filter="setFilters"></coach-filter>
    </section>
    <section>
    <base-card>
        <div class="controls">
        <base-button mode="outline" @click="loadCoaches">Refresh</base-button>
        <base-button  v-if="!isCoach" link to= "/register">Register as Coach</base-button>
        </div>
        <!-- <div v-if="isLoading">
            <base-spinner></base-spinner>
        </div> -->
        <ul v-if="hasCoaches">
           <coach-item v-for="coach in filteredCoaches" 
           :key="coach.id"
           :id="coach.id"
           :first-name="coach.firstName"
           :last-name="coach.lastName"
           :rate="coach.hourlyRate"
           :areas="coach.areas"
           ></coach-item>
        </ul>
        <h3 v-else>there is no any coaches</h3>
      </base-card>
    </section>
</template>
<script>
import { is, throwStatement } from '@babel/types';
import CoachItem from '@/components/coaches/CoachItem.vue';
// import BaseButton from '@/components/UI/BaseButton.vue';
import CoachFilter from '@/components/coaches/CoachFilter.vue';
import BaseSpinner from '@/components/UI/BaseSpinner.vue';

export default{
    components: {CoachItem, CoachFilter, BaseSpinner},
    data(){
        return{
            isLoading:false,
            activeFilters:{
                frontend: true,
                backend: true,
                career: true
            }
        }
    },
    computed: {
        isCoach(){
           return this.$store.getters['coaches/isCoach']
        },
        filteredCoaches(){
            const coaches = this.$store.getters['coaches/coaches']
            return coaches.filter(coach =>{
                if(this.activeFilters.frontend && coach.areas.includes('frontend')){
                    return true;
                }
                if(this.activeFilters.backend && coach.areas.includes('backend')){
                    return true
                }
                if(this.activeFilters.career && coach.areas.includes('career')){
                    return true;
                }
                return false

            })
        },
        hasCoaches(){
            return this.$store.getters['coaches/hasCoaches']
        }
    },
    created(){
      this.loadCoaches()
    },
    methods:{
        setFilters(updatedFilters){
            this.activeFilters = updatedFilters
        },
        loadCoaches(){
            // this.isLoading = true
            this.$store.dispatch('coaches/loadCoaches')
        //    this.isLoading = false 
        }
    }
   
}
</script>
<style scoped>
ul{
    list-style: none;
    margin:0;
    padding:0;
}
.controls{
    display:flex;
    justify-content: space-between;
    margin-bottom: 13px;
}
</style>