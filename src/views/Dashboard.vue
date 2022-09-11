<template>
  <div class="container">
    <div class="topBar">
      <h1>Load Profiles</h1>
      <p>Available datasets</p>
    </div>
    <div class="sectionContainer">
      <div class="profileList">
        <h6>Name</h6>
        <hr />
        <div
          v-for="profile in profiles"
          :key="profile">
                  <h2>{{profile.name}}</h2>
                  <p>{{profile.value +' MWh'}}</p>
                  <hr />
            </div>
      </div>
      <div class="rightSection">
        <div class="rightSectionHeader">
          <h1>Section Label</h1>
          <p>Load, daily view</p>
          <h3>kW</h3>
        </div>
        <ChartSection />
      </div>
      
    </div>
  </div>
</template>

<script>
import ChartSection from '../components/ChartSection.vue';
import profiles from '../data/load-profiles.json'

export default {
  name: 'Dashboard',

  data(){
    return {
      profiles,
    }
  },

  components: {
    ChartSection,
  },
}
</script>

<style scoped>
.container {
  background-color: #102D4C;
  padding: 0px 30px 40px 30px;
  font-family: Helvetica;
}

.topBar {
  color: #ffffff;
  display: flex;
  flex-direction: row;
  align-items: baseline;
  gap: 20px
}

.sectionContainer{
  display: flex;
  flex-direction: row;
  gap: 5px;
  text-align: left;
}

.profileList {
  background-color: #F1F1F1;
  width: 300px;
}

.profileList h2 {
  font-size: 14px;
  font-weight: bold;
  color: #2B3E52;
  padding: 0px 20px 0px 20px;
}

.profileList h6 {
  font-size: 10px;
  font-weight: bold;
  color: #2B3E52;
  padding: 0px 20px 0px 20px;
}

hr {
  border-top: 2px solid white;
}

.profileList p {
  font-size: 12px;
  color: #2B3E52;
  padding-left: 20px;

}

.rightSection{
  background-color: #F1F1F1;
  padding: 0px 15px 0px 15px;
  color: #102D4C;
}

.rightSectionHeader h1{
  font-size: 26px;
  font-weight: bold;
}

.rightSectionHeader p{
  font-size: 18px;
  margin-top: -20px;
}

.rightSectionHeader h3{
  font-size: 16px;
  font-weight: bold;
  margin-bottom: -0px;
}

</style>
