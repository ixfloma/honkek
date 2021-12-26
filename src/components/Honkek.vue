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
            <input type="number" name="currentCrystal" id="currentCrystal" v-model="currentCrystal" aria-label="Current Crystal" placeholder="0" class="form-control input-lg" min="0">
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
              <input type="number" name="monthlyProgress" id="monthlyProgress" class="form-control" v-model="monthlyProgress" placeholder="0" min="1" max="15">
              <div class="input-group-text">/15</div>
            </div>
          </div>
          <div class="my-3 mx-3">
            <label for="deadline" class="form-label">Target End Date</label>
            <input type="date" name="deadline" id="deadline" v-model="deadline" class="form-control input-lg">
          </div>
        </form>
      </div>
      <div class="col-lg-5 my-3">
        <table class="table">
          <thead>
            <tr>
              <th scope="col">Source</th>
              <th scope="col">Income</th>
              <th scope="col">Total</th>
            </tr>
          </thead>
          <tbody>
            <tr>
              <td>Daily</td>
              <td>40</td>
              <td>{{dailyRewards}}</td>
            </tr>
            <tr>
              <td>Memorial Arena</td>
              <td>{{memorial}}</td>
              <td>{{memorialArenaReward}}</td>
            </tr>
            <tr>
              <td>Abyss</td>
              <td>{{abyssRank}}</td>
              <td>{{abyssTotalReward}}</td>
            </tr>
            <tr v-if="monthlyCheck">
              <td>Monthly</td>
              <td>60 and 500 each 15 days</td>
              <td>{{monthlyTotal}}</td>
            </tr>
            <tr v-if="activeArmada">
              <td>Armada</td>
              <td>25</td>
              <td>{{armadaReward}}</td>
            </tr>
          </tbody>
        </table>
        <p>Total crystal get before {{deadline}} = <b>{{totalAllComputed}}</b></p>
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
      today: '',
      monthlyCheck: false,
      activeArmada: true,
      currentCrystal : '',
      monthlyProgress: '',
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
  mounted(){
    document.getElementById('deadline').min = this.today;
  },
  methods : {
    setTodayDate(){
      const date = moment(new Date).format('YYYY-MM-DD')
      this.deadline = date
      this.today =  date
    },
    bracketChange(){
      if(this.bracket !== 'Exalted'){
        this.memorial = '90'
      } else {
        this.memorial = '100'
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
    underSeven(todayA,diff){
      // const todayA = moment(this.today).day()
      var cycle = 0;
      if(diff == 6){
        cycle += 2;
      } else
      if(todayA == 4 && diff > 2){
        cycle +=1;
      } else
      if(todayA == 1 && diff > 1){
        cycle +=1
      } else
      if(todayA == 2){
        if(diff == 5){
          cycle +=2
        } else if (diff > 0) {
          cycle +=1
        }
      } else
      if(todayA == 3){
        if(diff == 4){
          cycle +=2
        } else if (diff > 4) {
          cycle +=1
        }
      } else
      if(todayA == 5 && diff > 1){
        if(diff == 5){
          cycle+=2
        } else {
          cycle+=1
        }
      } else
      if(todayA == 6 && diff > 0){
        if(diff == 4){
          cycle+=2
        } else {
          cycle+=1
        }
      } else
      if(todayA == 0){
        if(diff > 2){
          cycle+=2
        } else {
          cycle+=1
        }
      }

      return cycle;
    }
  },
  computed: {
    diffDays: function(){
      const today = moment(this.today)
      const deadline = moment(this.deadline)
      var diff = moment.duration(deadline.diff(today))
      return diff.asDays()
    },
    dailyRewards: function(){
      return 40 * this.diffDays
    },
    abyssTotalReward: function(){
      const today = moment(this.today).day()
      var cycle = 0;
      if(this.diffDays < 7){
        cycle = this.underSeven(today,this.diffDays)
      } else {
        var week = this.diffDays / 7;
        cycle = Math.floor(week) * 2;
        if(this.diffDays % 7 != 0){
          cycle = cycle + this.underSeven(today,this.diffDays%7)
        }
        if(today == '0' || today == '3'){
          cycle+=1
        }
      }
      return cycle * this.abyssRank
    },
    memorialArenaReward: function(){
      const today = moment(this.today).day()
      var cycle = 0;
      if(this.diffDays < 7){
        if(today == 1 && this.diffDays > 1){
          cycle+=1;
        }
        if(today == 2 && this.diffDays > 0){
          cycle+=1;
        } 
        if(today == 3){
          cycle+=1
        }
        if(today == 4 && this.diffDays == 6){
          cycle+=1
        }
        if(today == 5 && this.diffDays > 4){
          cycle+=1
        }
        if(today == 6 && this.diffDays > 3){
          cycle+=1
        }
        if(today == 0 && this.diffDays > 2){
          cycle+=1
        }
      } else {
        var week = this.diffDays / 7
        cycle = Math.floor(week)
        if(today == 3){
          cycle+=1
        }
        if(today == 0 || today > 3){
          if(this.diffDays % 7 != 0){
            if(moment(this.deadline).day() <= 3){
              cycle+=1
            }
          }
        } else {
          if(today =! 0 && today < 3){
            if(moment(this.deadline).day() <= 3){
              cycle+=1
            }
          }
        }
      }

      return cycle * this.memorial
    },
    minmaxmonthly: function(){
      if(this.monthlyProgress > 15){
        this.monthlyProgress = this.monthlyProgress % 15
      } else if(this.monthlyProgress < 0 ){
        this.monthlyProgress = 0
      }
    },
    armadaReward: function(){
      const today = moment(this.today).day()
      var cycle = 0;
      if(this.diffDays < 7){
        if(today == 1){
          cycle+=1;
        }
        if(today == 2 && this.diffDays == 6){
          cycle+=1;
        } 
        if(today == 3 && this.diffDays > 4){
          cycle+=1
        }
        if(today == 4 && this.diffDays > 3){
          cycle+=1
        }
        if(today == 5 && this.diffDays > 2){
          cycle+=1
        }
        if(today == 6 && this.diffDays > 1){
          cycle+=1
        }
        if(today == 0 && this.diffDays > 0){
          cycle+=1
        }
      } else {
        var week = this.diffDays / 7
        cycle = Math.floor(week)
        if(today == 1){
          cycle+=1
        }
        if(today == 0 || today > 1){
          if(this.diffDays % 7 != 0){
            if(moment(this.deadline).day() >= 1){
              cycle+=1
            }
          }
        }
      }

      return cycle * 25
    },
    monthlyTotal: function(){
      var total = this.diffDays * 60
      var cycletotal = 0;
      if(this.monthlyProgress == 15){
        cycletotal = Math.floor(this.diffDays / 15) * 500
        total = cycletotal + total
      } else {
        var totalDays = this.diffDays + this.monthlyProgress
        cycletotal = Math.floor(totalDays / 15) * 500
        total = cycletotal + total
      }

      return total
    },
    totalAllComputed: function(){
      var total = this.dailyRewards + this.abyssTotalReward + this.memorialArenaReward
      if(this.activeArmada){
        total = total + this.armadaReward
      }
      if(this.monthlyCheck){
        total = total + this.monthlyTotal
      }
      total = total + this.currentCrystal
      return total
    }
  }
}

</script>

<style scoped>

/* Chrome, Safari, Edge, Opera */
#currentCrystal::-webkit-outer-spin-button,
#currentCrystal::-webkit-inner-spin-button {
  -webkit-appearance: none;
  margin: 0;
}

/* Firefox */
#currentCrystal {
  -moz-appearance: textfield;
}
</style>
