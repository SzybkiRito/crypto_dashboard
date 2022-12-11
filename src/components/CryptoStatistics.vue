<template>
    <div class="crypto__stats--container">
        <nav>
            <ul class="crypto__stats--nav">
                <li><a href="">Summary</a></li>
                <li><a href="">Table</a></li>
                <li><a href="">Charts</a></li>
                <li><a href="">Reporting</a></li>
                <li><a href="">Analysis</a></li>
            </ul>
        </nav>

        <div class="crypto__summary--stats">
            <div class="crypto__wrapper" v-for="element in api_data" :key="element">
                <img height="48" width="48" :src="element.image" alt="crypto symbol" class="crypto_wrapper--icon" />
                <div class="crypto__wrapper--text">
                    <span class="crypto_short_title title">{{ element.name }}</span>
                    <span class="crypto_title description">{{ element.symbol }}</span>
                </div>
                <div class="crypto__wrapper--text">
                    <span class="crypto_price_title title">Price</span>
                    <span class="crypto_price description">${{ element.quote.USD.price.toFixed(3) }}</span>
                </div>
                <div class="crypto__wrapper--text">
                    <span class="crypto_change_title title">Change</span>
                    <span class="crypto_change description">{{ element.quote.USD.percent_change_24h.toFixed(2)
                    }}%</span>
                </div>
                <div class="crypto__wrapper--chart">
                    <LineChart class="crypto__wrapper--lineChart" :chartData="element.chartData" :options="options"
                        style="height: 100px;" />
                </div>
                <div class="crypto__wrapper--buttons">
                    <button class="crypto__wrapper--button crypto_balance_button--violet">Quick Invest</button>
                    <button class="crypto__wrapper--button crypto_balance_button--white">Show Report</button>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import { onMounted, ref } from "vue";
import { LineChart } from 'vue-chart-3';
import { Chart, registerables } from "chart.js";
Chart.register(...registerables);

export default {
    name: 'CryptoStatistics',
    components: { LineChart },
    setup() {
        const BASE_URL = 'https://cors-anywhere.herokuapp.com/https://pro-api.coinmarketcap.com/';
        let api_data = ref(null);
        let chartData = ref()
        let options = ref()

        function fetchAPIData() {
            fetch(`${BASE_URL}v1/cryptocurrency/listings/latest?start=1&limit=10`, {
                method: 'GET',
                mode: 'cors',
                headers: {
                    "Content-Type": "application/json",
                    "Access-Control-Allow-Origin": "*",
                    "X-CMC_PRO_API_KEY": process.env.VUE_APP_COINMARKET_API_KEY
                },
            }).then(async (res) => {
                res.json().then((data) => {
                    api_data.value = data.data
                    let symbolRequiredData = []

                    api_data.value.forEach(async (element) => {

                        symbolRequiredData.push(element.symbol);

                        if (element.quote.USD.percent_change_24h < 0) {
                            let priceLast1h = Math.abs(element.quote.USD.price + (element.quote.USD.price * Math.abs(element.quote.USD.percent_change_1h)))
                            let priceLast24h = Math.abs(element.quote.USD.price + (element.quote.USD.price * Math.abs(element.quote.USD.percent_change_24h)))
                            let priceLast30d = Math.abs(element.quote.USD.price + (element.quote.USD.price * Math.abs(element.quote.USD.percent_change_30d)))
                            let priceLast90d = Math.abs(element.quote.USD.price + (element.quote.USD.price * Math.abs(element.quote.USD.percent_change_90d)))

                            element.chartData = {
                                labels: ['1', '2', '3', '4'],
                                datasets: [{
                                    data: [priceLast1h, priceLast24h, priceLast30d, priceLast90d],
                                    fill: false,
                                    borderColor: 'rgb(234, 77, 77)',
                                }],
                            }
                        } else {
                            let priceLast1h = element.quote.USD.price - (element.quote.USD.price * element.quote.USD.percent_change_1h)
                            let priceLast24h = element.quote.USD.price - (element.quote.USD.price * element.quote.USD.percent_change_24h)
                            let priceLast30d = element.quote.USD.price - (element.quote.USD.price * element.quote.USD.percent_change_30d)
                            let priceLast90d = element.quote.USD.price - (element.quote.USD.price * element.quote.USD.percent_change_90d)

                            element.chartData = {
                                labels: ['1', '2', '3', '4'],
                                datasets: [{
                                    data: [priceLast1h, priceLast24h, priceLast30d, priceLast90d],
                                    fill: false,
                                    borderColor: 'rgb(45, 199, 143)',
                                }],
                            }
                        }
                    })
                    fetchImagesFromAPI(symbolRequiredData)
                });
            });
        }

        function fetchImagesFromAPI(images) {
            fetch(`${BASE_URL}v2/cryptocurrency/info?symbol=${images.join(',')}`, {
                method: 'GET',
                mode: 'cors',
                headers: {
                    "Content-Type": "application/json",
                    "Access-Control-Allow-Origin": "*",
                    "X-CMC_PRO_API_KEY": process.env.VUE_APP_COINMARKET_API_KEY
                },
            }).then(async (res) => {
                res.json().then((data2) => {
                    api_data.value.forEach((element, index) => {
                        api_data.value[index].image = data2.data[`${element.symbol}`][0].logo;
                    });
                    // console.log(data2.data['ADA']);
                });
            });
        }

        options.value = {
            plugins: {
                legend: {
                    display: false
                },
            },
            elements: {
                point: {
                    radius: 0
                },
            },
            scales: {
                y: {
                    display: false
                },
                x: {
                    display: false
                }
            },
            responsive: true,
            maintainAspectRatio: false,
            tension: 0.4
        }


        onMounted(() => {
            fetchAPIData();
        });

        return {
            api_data,
            chartData,
            options,
        }
    }
}
</script>

<style scoped>
.crypto__stats--container {
    background: var(--light-gray-background);
    border-radius: 16px;

    display: flex;
    flex-direction: column;
    flex-wrap: wrap;
}

.crypto__stats--nav a {
    color: inherit;
    text-decoration: none;
    font-weight: 500;
    font-size: 16px;
}

.crypto__stats--nav {
    display: flex;
    gap: 15px;
    margin-top: 30px;
    list-style-type: none;
}

.crypto__stats--nav a {
    display: block;
    white-space: nowrap;
}

.crypto__stats--nav a::after {
    display: block;
    content: '';
    border-bottom: 3px solid var(--violet);
    transform: scaleX(0);
    transition: transform 300ms ease-in-out;
}

.crypto__stats--nav a:hover::after {
    transform: scaleX(1);
}

.crypto__stats--nav a:active::after {
    transform: scaleX(1);
}


.crypto__wrapper {
    display: flex;
    align-items: center;
    justify-content: space-between;
    background: #fff;
    border-radius: 15px;
    padding: 20px;
    margin: 20px;
}

.crypto__wrapper--text {
    display: flex;
    flex-direction: column;
    width: 100px;
}

.title {
    color: var(--dark-gray-text);
    font-weight: 400;
}

.description {
    font-weight: 600;
    color: var(--black-text);
}

.crypto__wrapper--button {
    border: none;
    height: 40px;
    width: 109px;

    font-weight: 600;
    font-size: 14px;
    border-radius: 4px;
    margin-left: 10px;
    transition: 0.5s ease-in-out;
}

.crypto__wrapper_chart {
    display: flex;
    align-items: center;
    justify-content: center;
}


@media only screen and (max-width: 800px) {
    .crypto__wrapper {
        width: 90%;
        padding: 10px;
        margin: 10px;
    }

    .crypto__wrapper--chart {
        display: none;
    }

    .crypto__wrapper--text {
        text-align: center;
    }

    .crypto__wrapper--button {
        margin-bottom: 10px;
        margin-right: -50px;
    }

}
</style>