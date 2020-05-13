<template>
    <div class="container">
     <h1 class="title">{{ msg }}</h1>
     <div class="results">
       <p>Total games played: <strong>{{totalGames}}</strong></p>
       <p>Total buyins: <strong>{{totalBuins}}</strong></p>
       <p>Avg buyins: <strong>{{avgBuyins}}</strong></p>
       <p>Total cashed: <strong>{{totalCashed}}</strong></p>
       <p>Profit: <strong>{{profit}}</strong></p>
     </div>
     <div class="columns">
       <div class="column is-one-third">
         <div class="columns">
           <div class="column is-half">
             <h4 class="title">T$</h4>
             <template v-for="item in tournamentDollars.buyins" >
                <div :key="item" class="buttons ">
                 <b-button @click="addTourney('notReg', item)" class="is-fullwidth " type="is-primary"><strong>${{item}}</strong></b-button>
              </div>
             </template>
            </div> 
            <div class="column is-half">
              <h4 class="title">Normal</h4>
               <template v-for="item in tournamentRegulars.buyins" >
                <div :key="item" class="buttons ">
                 <b-button @click="addTourney('reg', item)" class="is-fullwidth " type="is-info"><strong>${{item}}</strong></b-button>
              </div>
             </template>
            </div>
           </div>
         </div>
       <div class="column tables">
        <h4 class="title">T$ results</h4>
        <b-table
            :data="tournamentDollars.data"
            :bordered="isBordered"
            :striped="isStriped"
            :narrowed="isNarrowed"
            :hoverable="isHoverable"
            :loading="isLoading"
            :focusable="isFocusable"
            :mobile-cards="hasMobileCards">

            <template slot-scope="props">
                <b-table-column field="id" label="#" width="40" numeric>
                    {{ props.row.id }}
                </b-table-column>

                <b-table-column field="buyin" label="Buyin">
                    ${{ props.row.buyin }}
                </b-table-column>
                
                <b-table-column field="test" label="Cashed" width="100">
                    <b-input v-model="tournamentDollars.cashed[props.row.id - 1]" ></b-input>
                </b-table-column>
            </template>
        </b-table>
        <h4 class="title">
          Regular results
        </h4>
        <b-table
            :data="tournamentRegulars.data"
            :bordered="isBordered"
            :striped="isStriped"
            :narrowed="isNarrowed"
            :hoverable="isHoverable"
            :loading="isLoading"
            :focusable="isFocusable"
            :mobile-cards="hasMobileCards">

            <template slot-scope="props">
                <b-table-column field="id" label="#" width="40" numeric>
                    {{ props.row.id }}
                </b-table-column>

                <b-table-column field="buyin" label="Buyin">
                    ${{ props.row.buyin }}
                </b-table-column>

                <b-table-column field="test" label="Cashed" width="100">
                    <b-input v-model="tournamentRegulars.cashed[props.row.id - 1]" ></b-input>
                </b-table-column>
            </template>
        </b-table>
       </div>
     </div>
    </div>
</template>

<script>
export default {
  name: 'PokerSession',
  props: {
    msg: String
  },
  data() {
            const tournamentDollars = {
              buyins: [1.1, 2.2, 5.5, 11, 22, 55, 109],
              data: [],
              totalWon: 0,
              cashed: [] 
            };
            const tournamentRegulars = {
              buyins: [3.3, 5.5, 7.5, 11, 22, 33, 55],
              data: [],
              totalWon: 0,
              cashed: []
            };

            return {
                isEmpty: false,
                isBordered: true,
                isStriped: true,
                isNarrowed: false,
                isHoverable: true,
                isFocusable: false,
                isLoading: false,
                hasMobileCards: true,
                tournamentDollars,
                tournamentRegulars
            };
        },
        methods: {
          addTourney(type, buyin) {
            if(type === 'reg'){
              this.tournamentRegulars.data.push({id: this.tournamentRegulars.data.length + 1, buyin, cashed: this.tournamentRegulars.cashed.push(0)});
            } else{
              this.tournamentDollars.data.push({id: this.tournamentDollars.data.length + 1, buyin, cashed: this.tournamentDollars.cashed.push(0)});
            }
          }
        },
        computed: {
          totalBuins: function() {
            let total = 0;
            this.tournamentDollars.data.forEach(item => {
              total += item.buyin;
            });
            this.tournamentRegulars.data.forEach(item => {
              total += item.buyin;
            });
            return total.toFixed(2);
          },
          totalCashed: function() {
            let total = 0.0;
            this.tournamentDollars.cashed.forEach(item => {
              total += parseFloat(item);
            });
            this.tournamentRegulars.cashed.forEach(item => {
              total += parseFloat(item);
            });
            return total.toFixed(2);
          },
          totalGames: function() {
            return this.tournamentDollars.data.length + this.tournamentRegulars.data.length;
          },
          avgBuyins: function() {
            let total = 0;
            this.tournamentDollars.data.forEach(item => {
              total += item.buyin;
            });
            this.tournamentRegulars.data.forEach(item => {
              total += item.buyin;
            });
            return (total/this.totalGames).toFixed(2);
          },
          profit: function() {
            return (this.totalCashed - this.totalBuins).toFixed(2);
          }
          
        }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->

