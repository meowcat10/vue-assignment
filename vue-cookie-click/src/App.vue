<script>
import Game from '@/components/Game.vue';

export default {
   name: 'Cookie Game',
   components: { Game },
   data() {
        return {
            game:{
                player:{
                    clicks:0, // Amount of Clicks (Default)
                    click_ratio:1, // Clicks pr cookie (increases with upgrades)
                    highest_score:0,
                    completedTasks:0,

                    upgrades:0,
                    cookie_skin:0,
                },

                shop:[
                    { upgrade: 'Double Click', ratio: 2, price: 50 },
                    { upgrade: 'Triple Click', ratio: 3, price: 100 },
                    { upgrade: 'Quadruple Click', ratio: 4, price:150 },
                    { upgrade: 'Quintuple', ratio: 5 , price: 200 },
                    { upgrade: 'Sextuple', ratio: 6, price: 300 },
                    { upgrade: 'Septuple', ratio: 7, price: 400 },
                    { upgrade: 'Octuple', ratio: 8, price: 600 },
                    { upgrade: 'Nonuple', ratio: 9, price: 850 },
                    { upgrade: 'Decuple', ratio: 10, price: 1000 }
                ],

                tasks:[
                    { task: 'Reach 50 clicks', goal: 50, reward: 50 },
                    { task: 'Reach 150 clicks', goal: 150, reward: 50 },
                    { task: 'Reach 250 clicks', goal: 250, reward: 75 },
                    { task: 'Reach 500 clicks', goal: 500, reward: 200 }
                    
                ]
            }

        }
    },
    props:{ },
    
    methods: {
        getData(){
            const player = this.game.player;
            const shop = this.game.shop;
            const tasks = this.game.tasks;
            return { player, shop, tasks };
        },
        userClick(){
            // Receive player-data from Game
            const { player, tasks } = this.getData();
            // Ratio applies upgrades (if player has any)
            player.clicks += player.click_ratio;

            // Checks if tasks are completed (Regardless of upgrades)
            if (tasks[player.completedTasks]){
                if (player.clicks >= tasks[player.completedTasks].goal) {
                    player.clicks+= tasks[player.completedTasks].reward;
                    tasks[player.completedTasks].task += ' (DONE)';
                    player.completedTasks++;
                }
            }
            

            // Update highest if new
            if (player.clicks > player.highest_score) {
                player.highest_score = player.clicks;
            }
            
        },

        buyUpgrade(){
            // Receive data from Game
            const { player, shop } = this.getData();
            const shopPrice = shop[player.upgrades].price;
                // If player has enough clicks for upgrade 
            if (player.clicks>=shopPrice){
                player.clicks -= shopPrice;
                player.click_ratio = shop[player.upgrades].ratio;
                player.upgrades++;
            } else { alert('Do you not have enough clicks!') }
        },
        skinChange(){
            // Ensures index stays within skin limit
            const getSkinIndex = this.getData().player;
            if (getSkinIndex.cookie_skin < 2){
                getSkinIndex.cookie_skin++;
            } else if (getSkinIndex.cookie_skin == 2){
                getSkinIndex.cookie_skin = 0;
            }
        }
    }
}
</script>

<template>
  <main class="gameContainer">

    <!-- Side -->
    <section class="sidebar">
      <div class="sidebarTitle">
        <h3 title="Made using Vue">Cookie Clicker</h3>
        <p>Highest: {{ getData().player.highest_score }}</p>
      </div>
      <section id="shop">
        <div class="btnContainer" v-if="getData().player.upgrades < getData().shop.length">
          
          <!-- Shop (Button) -->
          <div class="btnRow">
            <div class="btn-text">
            <p>{{ getData().shop[getData().player.upgrades].upgrade }}<br>
            <span style="font-size:10px">Price: {{ getData().shop[getData().player.upgrades].price }}</span></p>
          </div>
          <button
            :class="{ 'not-enough': getData().player.clicks<getData().shop[getData().player.upgrades].price }"
            @click="buyUpgrade()">Buy</button>
          </div>

          <!-- Skin (Button) -->
          <div class="btnRow">
            <div class="btn-text">Skin</div>
            <button @click="skinChange()">Change</button>
          </div>
        </div>

        <!-- All unlocked / Cheats -->
        <div class="btnContainer" v-else>
          <p>You have all upgrades!</p>
          <button @click="getData().player.clicks+=100">Add 100</button>
          <button @click="getData().player.clicks-=100">Remove 100</button>
          <button @click="getData().player.clicks=0">Reset</button>
        </div>

        <!-- Tasks -->
        <ul>
          <li v-for="(task, index) in getData().tasks" :key="index">
            {{ task.task }}<br>
            <span style="font-size:10px">Reward: {{ task.reward }}</span>
          </li>
        </ul>
              
      </section>
    </section>

    <!-- Game (Props sent and Emit listener)-->
    <Game
        :click-ratio="getData().player.click_ratio"
        :clicks="getData().player.clicks"
        :cookie-skin="getData().player.cookie_skin"
        @user-click="userClick()"
    />

  </main>

</template>

<style scoped>

  .gameContainer{
    display:grid;
    grid-template-columns: 300px 500px;
    justify-content: center;
    #shop{ width: 85% }
    
    .sidebar{
      display:flex;
      flex-direction:column;
      align-items:center;
      background:var(--sidebarColor);

      .sidebarTitle{
        display:grid;
        grid-template-rows: auto 50px;
        justify-content: center;
        text-align: center;
        align-items:center;
        width:100%;
        max-height:125px;
        * {font-family: Arial, sans-serif !important; margin-bottom:4px; }
        p { font-size:10px; opacity: 0.5;}
        font-weight:bold;
        background:#95614b;
      }
      
      .btnContainer{
        display:flex;
        flex-direction:column;
        .btnRow{
          display:flex;
          align-items: center;
          justify-content: space-between;
          .btn-text{
            display:flex;
            flex-direction:row;
          }
          button{
            height:fit-content;
            color:#fff;
            background:rgb(62, 128, 21);
            min-width:var(--btnWidth);
            padding: var(--btnPadding);

            &.not-enough{
              background:#c0392b;
            }
            &:hover{
              cursor:pointer;
            }
          }
        }
      }
    }
  }
</style>