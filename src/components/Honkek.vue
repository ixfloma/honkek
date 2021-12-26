<template>
  <div class="navbar navbar-expand-lg navbar-light bg-light">
      <div class="container-fluid">
          <a href="#home" class="navbar-brand">Honkai Impact Calculator</a>
          <!-- <a href="#subscribe" class="nav-link float-end disabled">Hibiscia yeah</a> -->
      </div>
  </div>
  <div class="container-fluid">
    <div class="row g-2">
      <div class="forms col-lg-6 mx-3">
        <form>
          <div class="my-3 mx-3">
            <label for="currentCrystal" class="form-label">Current Crystal</label>
            <input type="number" name="currentCrystal" id="currentCrystal" v-model="currentCrystal" aria-label="Current Crystal" placeholder="0" class="form-control input-lg">
          </div>

          <div class="my-3 mx-3">
            <div class="row">
              <div class="col">
                <label for="bracket" class="form-label">Level Bracket</label>
                <select name="bracket" id="bracket" v-model="bracket" class="form-select" @change="bracketChange">
                  <option value="Exalted">Exalted (81+)</option>
                  <option value="Master">Master (Lv 70-80)</option>
                  <option value="Elite">Elite (Lv 56-69)</option>
                  <option value="Basic">Basic (Lv 38-55)</option>
                </select>
              </div>
              <div class="col">
                <label for="abyssRank" class="form-label">Dirac Sea/Abyss Rank</label>
                <select name="abyssRank" id="abyssRank" v-model="abyssRank" class="form-select">
                  <option v-for="(rank, index) in ranking" :value="rank.reward" :key="index">
                    {{rank.ranking}}
                  </option>
                </select>
              </div>
            </div>
          </div>
          <div class="my-3 mx-3">
            <div class="form-check my-1">
              <input v-model="activeArmada" class="form-check-input" type="checkbox" value="0" id="activeArmada">
              <label class="form-check-label" for="activeArmada">
                  Armada is Active
              </label>
            </div>
          </div>
          <div class="my-3 mx-3">
            <div class="form-check my-1">
              <input type="checkbox" name="monthlyCheck" id="monthlyCheck" value="0" class="form-check-input" v-model="monthlyCheck">
              <label for="monthlyCheck" class="form-check-label">Monthly Card Active</label>
            </div>
          </div>
          <div class="my-3 mx-3" v-if="monthlyCheck">
            <label for="monthlyProgress" class="form-label">Monthly Progress</label>
            <div class="input-group">
              <input type="text" name="monthlyProgress" id="monthlyProgress" class="form-control" v-model="monthlyProgress" placeholder="0">
              <div class="input-group-text">/15</div>
            </div>
          </div>
          <div class="my-3 mx-3">
            <label for="deadline" class="form-label">Target End Date</label>
            <input type="date" name="deadline" id="" v-model="deadline" class="form-control input-lg">
          </div>
        </form>
      </div>
    </div>
  </div>  
</template>

<script>
import moment from "moment"
export default{
  data(){
    return{
      deadline: '',
      monthlyCheck: false,
      activeArmada: true,
      currentCrystal : '',
      bracket: 'Exalted',
      abyssRank: '500',
      memorial: '100',
      ranking : [
        { ranking: 'Nirvana', reward: 520 },
        { ranking: 'Red Lotus', reward: 500},
        { ranking: 'Agony III', reward: 420},
        { ranking: 'Agony II', reward: 340 },
        { ranking: 'Agony I', reward: 280 },
        { ranking: 'Sinful III', reward: 220 },
        { ranking: 'Sinful II', reward: 200 },
        { ranking: 'Sinful I', reward: 190 },
        { ranking: 'Forbidden', reward: 180 },
      ]
    }
  },
  created(){
    this.setTodayDate();
  },
  methods : {
    setTodayDate(){
      this.deadline = moment(new Date).format('YYYY-MM-DD')
    },
    bracketChange(){
      if(this.bracket !== 'Exalted'){
        this.memorial = 90
      }
      if(this.bracket == 'Master'){
        this.abyssRank = 420,
        this.ranking = [
          { ranking: 'Red Lotus', reward: 420 },
          { ranking: 'Agony', reward: 260 },
          { ranking: 'Sinful', reward: 180 },
          { ranking: 'Forbidden', reward: 80 }
        ]
      } else if(this.bracket == 'Elite'){
        this.abyssRank = 350,
        this.ranking = [
          { ranking: 'Red Lotus', reward: 350 },
          { ranking: 'Agony', reward: 220 },
          { ranking: 'Sinful', reward: 140 },
          { ranking: 'Forbidden', reward: 70 }
        ]
      } else if(this.bracket == 'Basic'){
        this.abyssRank = 300,
        this.ranking = [
          { ranking: 'Red Lotus', reward: 300 },
          { ranking: 'Agony', reward: 180 },
          { ranking: 'Sinful', reward: 100 },
          { ranking: 'Forbidden', reward: 60 }
        ]
      } else if(this.bracket == 'Exalted'){
        this.abyssRank = 500,
        this.ranking = [
          { ranking: 'Nirvana', reward: 520 },
          { ranking: 'Red Lotus', reward: 500 },
          { ranking: 'Agony III', reward: 420 },
          { ranking: 'Agony II', reward: 340 },
          { ranking: 'Agony I', reward: 280 },
          { ranking: 'Sinful III', reward: 220 },
          { ranking: 'Sinful II', reward: 200 },
          { ranking: 'Sinful I', reward: 190 },
          { ranking: 'Forbidden', reward: 180 }
        ]
      }
    },
  }
}

</script>

<style scoped>
/* Chrome, Safari, Edge, Opera */
input::-webkit-outer-spin-button,
input::-webkit-inner-spin-button {
  -webkit-appearance: none;
  margin: 0;
}

/* Firefox */
input[type=number] {
  -moz-appearance: textfield;
}
</style>
