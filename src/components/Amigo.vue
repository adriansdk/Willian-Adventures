<template>
    <div class="ally">
        <div class="container-fluid">
            <!-- NAMES -->
            <div class="row">
                <div class="col-12">
                    <div class="flying d-flex justify-content-center">
                        <img v-if="heroHp == 0" class="angel" src="@/assets/spirit-healer.png" alt="">
                    </div>
                </div>
            </div>
            <div v-bind:css="{ 'darken': (heroHp == 0) }">
                <div class="row">
                    <div class="col logo"><img src="@/assets/logo.png" alt=""></div>
                </div>
                <div class="row">
                    <h1 class="display-1 col-lg-4"> {{ heroName }} </h1>
                    <div class="col-lg-4"> </div>
                    <h1 class="display-1 col-lg-4"> {{ enemyName }} </h1>
                </div>                  
                <!-- HP -->
                <div class="row">
                    <h3 class="col-lg-4 text-left hpBar">
                        <template v-if="heroHp > 0"> HP: {{ heroHp }} </template>
                        <template v-else> Bahh bicho, tu morreu </template>
                    </h3>  
                    <div class="col-lg-4"> </div>
                    <h3 class="col-lg-4 text-left hpBar">
                        <template v-if="enemyHp > 0"> HP:  {{ enemyHp }} </template>
                        <template v-else> Menos um borsa na quebrada </template>
                    </h3>  
                </div>
                <!-- IMAGES -->
                <div class="row">
                    <img v-if="enemyHp > 0" class="col-4" src="https://scontent.fcac4-1.fna.fbcdn.net/v/t1.0-9/28795537_10156741824394947_767611066692760925_n.jpg?_nc_cat=104&_nc_ht=scontent.fcac4-1.fna&oh=cd1597cc3123262b6b4bfc86824b87a8&oe=5D37D70E" width="400px" height="400px">
                    <img v-if="enemyHp <= 0"  class="col-4" src="@/assets/victorious-will.jpg" height="400px" width="100%">
                    <div class="col-4"> </div>
                    <img class="col-4" :src="enemies[currentEnemy]" width="400px" height="400px">   
                </div>

                <!-- STATS  -->
                <div class="row">
                    <div class="col-4">
                        <h3 class="text-left"> Damage: {{ heroDmg }} </h3>
                        <h3 class="text-left"> Defense: {{ heroDefense }} </h3>
                        <h3 class="text-left"> Evade Chance: {{ heroEvadeChance }}% </h3>
                        <h3 class="text-left"> Exp to Level UP: {{ exp }}/{{ expToLvl }} </h3>
                        <h3 class="text-left"> Gold: {{ heroGold }} </h3>
                        <h3 class="text-left"> Level: {{ level }} </h3>
                        <div class="row">
                        <button class="btn-info text-left actionsBtn col" v-if="enemyHp || heroHp <= 0" v-on:click="heroAttack(), enemyAttack()"> Hero Attack! </button>
                        <button class="btn-secondary text-left actionsBtn disabled col" v-else disabled> Para! Para! Ele já ta morto!</button>
                        <button v-if="enemyHp <= 0" v-on:click="nextFight()" class="actionsBtn btn btn-danger col"> Next fight! </button>
                        <button v-if="exp >= 100" v-on:click="levelUp()" class="actionsBtn btn btn-success col"> Level UP!! </button>   
                        </div>
                    </div>
                    <!--   STORE    -->
                    <div class="col-4 store">
                        
                    </div>
                    <!-- STORE END -->
                    <div class="col-4">
                        <h3 class="text-left"> Damage: {{ enemyDmg }} </h3>
                        <h3 class="text-left"> Defense: {{ enemyDefense}} </h3>
                        <h3 class="text-left"> Exp: {{ enemyExp }} </h3>
                        <h3 class="text-left"> Level: {{ enemyLevel }} </h3>
                    </div>
                </div>
                <!-- ACTIONS -->
                <div class="row">    
                </div>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    data() {
        return{
            heroName: "Will",
            heroDmg: 10,
            heroDefense: 2,
            heroHp: 25,
            heroGold: 0,
            baseHeroHp: 25,
            level: 0,
            expToLvl: 100,
            enemyName: "Rat",
            enemyHp: 14,
            enemyLevel: 0,
            enemyDmg: 5,
            enemyDefense: 3,
            enemyExp: 13,
            exp: 0,
            heroEvadeChance: 27,
            currentEnemy: 0,
            defense: 3,

            enemies: [
                'https://spectator.imgix.net/content/uploads/2018/05/iStock-182819758.jpg?auto=compress,enhance,format&crop=faces,entropy,edges&fit=crop&w=820&h=550',
                'https://pixel.nymag.com/imgs/daily/vulture/2018/04/25/25-thanos.w700.h700.jpg',
                'https://www.rbsdirect.com.br/imagesrc/21283533.jpg?w=700',
            ],
            enemiesHp: [
                14,
                35,
                28,
            ],
            enemiesDmg: [
                5,
                8,
                11,
            ],
            enemiesDefense: [
                3,
                7,
                10,
            ],
            enemiesExp: [
                40,
                20,
                26,

            ],
            enemiesLvl:[
                0,
                1,
                2,
                
            ],
            monsterLoot:[
                10,
                14,
                19,
            ],
         };
    },
    methods:{
        heroAttack: function() {
           if(this.enemyHp > 0 && this.heroHp > 0){ 
               let heroDamage = this.heroDmg - this.enemyDefense;
               this.enemyHp -= heroDamage;
           } 
           if (this.enemyHp <= 0){
               this.enemyHp = 0
           }
        },
        enemyAttack: function() {
            if(this.heroHp > 0){
                let enemyDamage = this.enemyDmg - this.heroDefense;
                if(this.evadeChance() > this.heroEvadeChance){
                     this.heroHp -= enemyDamage;
                }
            }
            if (this.heroHp <= 0){
               this.heroHp = 0
           }
        },
        evadeChance: function () {
            let min = Math.ceil(1);
            let max = Math.floor(100);
            return Math.floor(Math.random() * (max - min + 1)) + min;
         },
        nextFight: function(){
            if(this.enemyHp <= 0){
                if(this.level > this.enemyLevel){
                    this.currentEnemy++
                }
                this.enemyHp = this.enemiesHp[this.currentEnemy]
                this.enemyDmg = this.enemiesDmg[this.currentEnemy]
                this.enemyDefense = this.enemiesDefense[this.currentEnemy]
                this.enemyLevel = this.enemiesLvl[this.currentEnemy]
                this.exp += this.enemyExp
                this.enemyExp = this.enemiesExp[this.currentEnemy]
            }
        },
        levelUp: function(){
            if(this.exp >= this.expToLvl){   
               console.log(this.level)       
                this.heroDmg = this.heroDmg * 1.6;
                this.heroDefense = this.heroDefense * 1.3;
                this.heroEvadeChance = Math.round(this.heroEvadeChance  * 1.05);
                this.heroHp = this.baseHeroHp * 1.1
                this.exp = 0
                this.expToLvl = this.expToLvl + 30
                this.level = ++this.level
                console.log(this.level)
            }
        },
        deathPenalty: function(){
            if(heroHp == 0){
                
            }
        }
    },
}


</script>

<style scoped>

.logo{
    position: absolute;
}

.hpBar{
    background-color: red;
    border: black 5px solid;
}

.disabled{
    background-color: #333 !important;
}

.angel{
    z-index: 100;

 }

 .flying {
     position: absolute;
     width: 100%;  
 }

 .darken{
     color: rgba(0, 0, 0, 0.5);
 }

 .actionsBtn{
     height: 100px;
     font-size: 30px;
     font-family: 'Times New Roman', Times, serif;
     font-weight:bold;
     text-align: center;
 }

 .store{
     border: 5px solid black;
     background-color: aqua;
 }


</style>
