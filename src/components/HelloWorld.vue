<template>
    <div class="result" v-if="vikingLotto">
        <p
            v-for="gameResult in vikingLotto.gameResult"
            :key="gameResult.drawId"
        >
            {{ new Date(gameResult.drawDate).toLocaleDateString() }} -
            {{ sortData(gameResult.winnerNumber) }}
            <b>{{ gameResult.winnerNumber[6].number }}</b>
        </p>
    </div>
</template>

<script>
const axios = require('axios');

export default {
    name: 'HelloWorld',
    data: function() {
        return {
            vikingLotto: null,
        };
    },
    methods: {
        async getData() {
            return await axios
                .get(
                    'https://api.norsk-tipping.no/LotteryGameInfo/v1/api/results/vikinglotto',
                    {
                        params: {
                            fromDate: '2019-01-01',
                            toDate: `${new Date().getFullYear()}-${new Date().getMonth() +
                                1}-${new Date().getDate()}`,
                        },
                    }
                )
                .then(function(response) {
                    console.log(response.data);
                    console.log(this.vikingLotto);

                    this.vikingLotto = response.data;
                    // return response;
                })
                .catch(function(error) {
                    console.log(error);
                });
        },
        sortData: (item) => {
            let numbers = [];

            item.forEach((i) => {
                if (i.type == 1) {
                    numbers.push(i.number);
                }
            });
            return numbers.sort((a, b) => a - b).toString();
        },
    },
    mounted: function() {
        // console.log(this.getData());

        // this.vikingLotto = this.getData();
        axios
            .get(
                'https://api.norsk-tipping.no/LotteryGameInfo/v1/api/results/vikinglotto',
                {
                    params: {
                        fromDate: '2019-01-01',
                        toDate: `${new Date().getFullYear()}-${new Date().getMonth() +
                            1}-${new Date().getDate()}`,
                    },
                }
            )
            .then((response) => (this.vikingLotto = response.data));
    },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
p,
.result {
    margin: 0;
    padding: 0;
}
p {
    margin-bottom: 0.3em;
}
</style>
