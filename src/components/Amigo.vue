<template>
    <div class="ally">
        <div class="container-fluid">
            <!-- NAMES -->
            <div class="row">
                <!-- <div class="col-12">
                    <div class="flying d-flex justify-content-center">
                        
                    </div>
                </div> -->
            </div>
            <div v-bind:css="{ 'darken': (heroHp == 0) }">
                <div class="row">
                    <div class="col logo"><img src="@/assets/logo.png" alt=""></div>
                </div>
                <div class="row">
                    <h1 class="display-1 col-lg-4"> {{ heroName }} </h1>
                    <div class="col-lg-4">  </div>
                    <h1 class="display-1 col-lg-4"> {{ enemyName[currentEnemy] }} </h1>
                </div>                  
                <!-- IMAGES -->
                <div class="row">
                    <img v-if="enemyHp > 0" class="col-4" src="@/assets/will.png" width="400px" height="400px">
                    <img v-if="enemyHp <= 0"  class="col-4" src="@/assets/victorious-will.png" height="400px" width="100%">
                    <div class="col-4"> 
                        <div class="row">
                            <img v-if="heroHp == 0" class="angel" src="@/assets/spirit-healer.png" height="250px" width="100%" alt=""> 
                            <img v-else-if="ressurect()" class="angel" src="@/assets/spirit-healer.png" height="250px" width="100%" alt=""> 
                        </div>
                        <div v-if="heroHp == 0" class="row">
                            <h3>{{ deathMessage }}</h3>
                        </div>
                        <div class="row">
                            <button v-if="heroHp == 0 && clicks < 3" v-on:click="ressurect(0)" class="btn btn-light col"> Ressurect </button>
                            <button v-if="heroHp == 0 && clicks < 3" v-on:click="ressurect(1)" class="btn btn-dark col"> Complain </button>
                            <button v-if="clicks >= 3" v-on:click="ressurect(2)" class="btn btn-dark col"> Thanks Nah! </button>
                        </div>
                    </div>
                    <img class="col-4" :src="enemies[currentEnemy]" width="400px" height="400px">   
                </div>
                <!-- HP -->
                <div class="row">
                    <h3 class="col-lg-4 text-left">
                        <b-progress class="mt-2 hpBar" :max="maxHeroHp" show-value>
                            <b-progress-bar :value="counter" variant="success">HP:{{ heroHp }}</b-progress-bar>
                        </b-progress>
                    </h3>  
                    <div class="col-lg-4 stats d-flex justify-content-between"> 
                        <button class="btn-secondary" v-on:click="buyItem(0)">Buy <br>Wireless Mouse<br> {{ itemValue[0] }} GOLD</button>
                        <button class="btn-secondary" v-on:click="buyItem(1)">Buy <br>Huge Monitor<br> {{ itemValue[1] }} GOLD</button>
                        <button class="btn-secondary" v-on:click="buyItem(2)">Buy <br>MacBook Pro<br> {{ itemValue[2] }} GOLD</button>
                        <button class="btn-secondary" v-on:click="buyItem(3)">Buy <br>Developer Skills<br> {{ itemValue[3] }} GOLD</button>
                     </div>
                    <h3 class="col-lg-4 text-left">
                        <b-progress class="mt-2 hpBar" :max="maxEnemiesHp[currentEnemy]" show-value>
                            <b-progress-bar :value="counter2" variant="danger">HP: {{ enemyHp }}</b-progress-bar>
                        </b-progress>
                    </h3>  
                </div>

                <!-- STATS  -->
                <div class="row stats">
                    <div class="col-4">
                        <h3 class="text-left"> Damage: {{ heroDmg }} </h3>
                        <h3 class="text-left"> Defense: {{ heroDefense }} </h3>
                        <h3 class="text-left"> Evade Chance: {{ heroEvadeChance }}% </h3>
                        <h3 class="text-left"> Exp to Level UP: {{ currentExp }}/{{ expToLvl }} </h3>
                        <h3 class="text-left"> Gold: {{ heroGold }} </h3>
                        <h3 class="text-left"> Level: {{ level }} </h3>
                        <div class="row">
                        <button class="btn-info text-left actionsBtn col" v-if="enemyHp || heroHp <= 0" v-on:click="heroAttack(), enemyAttack()"> Hero Attack! </button>
                        <button class="btn-secondary text-left actionsBtn disabled col" v-else disabled> Stop! he's already dead.</button>
                        <button v-if="enemyHp <= 0" v-on:click="nextFight(), loot()" class="actionsBtn btn btn-danger col"> Next fight! </button>
                        <button v-if="currentExp >= expToLvl" v-on:click="levelUp()" class="actionsBtn btn btn-success col"> Level UP!! </button>   
                        </div>
                    </div>
                    <!--   STORE    -->
                    <div class="col-4 store">
                        <h6 class="mx-auto display-3">LORE</h6>
                        <h3 class="mx-auto lore"> {{ lore[currentEnemy] }}</h3>
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
            max: 25,
            heroName: "Will",
            heroDmg: 10,
            heroDefense: 2,
            heroHp: 25,
            baseHeroHp: 25,
            maxHeroHp: 25,
            heroGold: 0,
            level: 0,
            currentExp: 0,
            expToLvl: 100,
            enemyHp: 14,
            enemyLevel: 0,
            enemyDmg: 5,
            enemyDefense: 3,
            enemyExp: 13,
            exp: 0,
            heroEvadeChance: 27,
            currentEnemy: 0,
            defense: 3,
            deathMessage: "Ohh dude, it so happens that you died, but i can ressurect you if you want, but you'll lose 10% xp, and if you complain you'll lose 20%",
            clicks: 0,
            lore:[
                "The infamous Rat, from Ratatouille, kept picking on Will throught his Chef career, pulling his hair and causing major issues at the restaurant where he worked.",
                "Thanos, probably Willian's most hated Marvel villain, simply because he has absolutelly amazing posture, Willian, as a Web Developer feels personally atacked",
                "Rocky, ohh Rocky... Their fights can only be described as the two main characters in an anime, where they are natural arch enemys for no distinguishable reason",
                "Baco, the ultimate battle, this magnificient beast was the only enemy Willian didn't stand a chence, mainly because he's too much of a good guy to kick a dog.",
            ],
            enemyName: [
                'Rat',
                'Thanos',
                'Rocky',    
                'Baco',
            ],
            itemBonus:{
                dmg:[
                    5,
                    8,
                    11,
                    30,
                ],
                dfs:[
                    4,
                    7,
                    9,
                    12,            
                ],

                hp: [
                    8,
                    11,
                    23,
                    40,
                ],

            },

            itemValue: [
                100,
                300,
                500,
                1000,
            ],
            enemies: [
                'http://miam-images.m.i.pic.centerblog.net/08966105.png',
                'https://i.redd.it/ogxoh09zpcu11.png',
                'http://www.pngonly.com/wp-content/uploads/2017/05/Rock-Small-PNG.png',
                'https://banner2.kisspng.com/20180730/cuo/kisspng-border-collie-english-shepherd-dog-breed-german-sh-red-border-collie-5b5ec8b93b7a80.3462849615329384252436.jpg',
            ],
            maxEnemiesHp: [
                14,
                35,
                28,
                300,
            ],
            enemiesHp: [
                14,
                35,
                28,
                300,
            ],
            enemiesDmg: [
                5,
                8,
                11,
                30,
            ],
            enemiesDefense: [
                3,
                7,
                10,
                25,
            ],
            enemiesExp: [
                40,
                20,
                26,
                100,
            ],
            enemiesLvl:[
                0,
                1,
                2,
                3,
            ],
            monsterLoot:[
                10,
                14,
                23,
                35,
                300,
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
           this.mathRound()
        },
        enemyAttack: function() {
            if(this.heroHp > 0){
                let enemyDamage = this.enemyDmg - this.heroDefense;
                if(this.chance() > this.heroEvadeChance){
                     this.heroHp -= enemyDamage;
                }
            }
            if (this.heroHp <= 0){
               this.heroHp = 0
           }
           this.mathRound()
        },

        loot: function (chance) {
            if (this.enemyHp > 0){
                let chance = this.chance()
                if(chance <= 35){
                    this.heroGold += this.monsterLoot[0]
                }else if(chance > 40 && chance < 60){
                    this.heroGold += this.monsterLoot[1]
                }else if(chance >= 60 && chance < 85){
                    this.heroGold += this.monsterLoot[2]
                }else{
                    this.heroGold += this.monsterLoot[3]
                }
            }
            this.mathRound()
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
                this.currentExp += this.enemyExp
                this.enemyExp = this.enemiesExp[this.currentEnemy]
            }
            this.mathRound()
        },
        levelUp: function(){
            if(this.currentExp >= this.expToLvl){       
                this.heroDmg = this.heroDmg * 1.6;
                this.heroDefense = this.heroDefense * 1.3;
                this.heroEvadeChance = Math.round(this.heroEvadeChance  * 1.05);
                this.maxHeroHp = Math.round(this.baseHeroHp * 1.1)
                this.heroHp = Math.round(this.maxHeroHp)
                this.currentExp = this.currentExp - this.expToLvl
                this.expToLvl += 30
                this.level = ++this.level
            }
            this.mathRound()
        },
        buyItem: function(item){
            if(this.heroGold >= this.itemValue[item]){
                this.heroGold -= parseInt(this.itemValue[item])
                this.heroDmg += this.itemBonus.dmg[item]
                this.heroDefense += this.itemBonus.dfs[item]
                this.heroHp += this.itemBonus.hp[item]
            }
            this.mathRound()
        }, 
        chance: function () {
            let min = Math.ceil(1);
            let max = Math.floor(100);
            return Math.floor(Math.random() * (max - min + 1)) + min;
        },
        ressurect: function(btn){
            if(btn == 0){
                this.currentExp *= 0.9
                this.heroHp = this.maxHeroHp
            }else if (btn == 1){
                this.currentExp *= 0.8
                this.clicks++
                this.heroHp = this.maxHeroHp
            }else if (btn == 2 && this.clicks >= 3){
                this.deathMessage = "Ok Willian, you're the most annoying person in the world, you'll gain 10% Exp'"
                this.currentExp *= 1.1
                this.heroHp = this.maxHeroHp
                this.clicks == 0
            }
            this.mathRound()
        },
        mathRound:function(){
            this.heroDmg = Math.round(this.heroDmg)
            this.heroHp = Math.round(this.heroHp)
            this.heroDefense = Math.round(this.heroDefense)
            this.heroEvadeChance = Math.round(this.heroEvadeChance)
            this.currentExp = Math.round(this.currentExp)
        },
    },
    computed:{
        counter(){
            return this.heroHp
        },
        counter2(){
            return this.enemyHp
        }
    }
}


</script>

<style scoped>

body{
    font-family: 'Oswald';
     font-weight:400;
}

.angel{
    z-index: 100;
}

.logo{
    position: absolute;
}

.hpBar{
    height: 60px;
    background-color: rgba(192, 189, 189, 0.685);
    border: 2px black solid;
    border-radius: 15px;
    font-family: 'Oswald', sans-serif;
    font-size: 30px;
}

.disabled{
    background-color: #333 !important;
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
     text-align: center;
 }

 .btn{
     border: 2px solid black;
     border-radius: 15px;
 }

 .stats{
     font-family: 'Oswald', sans-serif;
     font-weight: 400;
 }

 .lore{
     font-weight: 800;
 }


</style>
