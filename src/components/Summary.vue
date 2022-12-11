<template>
    <div class="crypto__summary--container">
        <div class="crypto__summary--texts">
            <h3 class="crypto__summary--title">Summary</h3>
            <button class="options_button"><svg width="18" height="4" viewBox="0 0 18 4" fill="none"
                    xmlns="http://www.w3.org/2000/svg">
                    <path
                        d="M9 3C9.55228 3 10 2.55228 10 2C10 1.44772 9.55228 1 9 1C8.44772 1 8 1.44772 8 2C8 2.55228 8.44772 3 9 3Z"
                        stroke="#9896A1" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" />
                    <path
                        d="M16 3C16.5523 3 17 2.55228 17 2C17 1.44772 16.5523 1 16 1C15.4477 1 15 1.44772 15 2C15 2.55228 15.4477 3 16 3Z"
                        stroke="#9896A1" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" />
                    <path
                        d="M2 3C2.55228 3 3 2.55228 3 2C3 1.44772 2.55228 1 2 1C1.44772 1 1 1.44772 1 2C1 2.55228 1.44772 3 2 3Z"
                        stroke="#9896A1" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" />
                </svg>
            </button>
        </div>

        <LineChart :chartData="chartData" :options="options" style="height: 189px; margin: 0px 32px 32px 32px" />

    </div>
</template>

<script>
import { LineChart } from 'vue-chart-3';
import { Chart, registerables } from "chart.js";
Chart.register(...registerables);

export default {
    name: 'Summary',
    components: { LineChart },
    setup() {

        function getMonthName(monthNumber) {
            const date = new Date();
            date.setMonth(monthNumber - 1);

            // Using the browser's default locale.
            return date.toLocaleString([], { month: 'long' });
        }

        function getDaysInCurrentMonth() {
            let date = new Date();
            let currentMonth = date.getMonth() + 1
            let numDays = new Date(date.getFullYear(), currentMonth, 0).getDate();
            let days = [];

            for (var i = 1; i <= numDays; i++) {
                days.push(`${i} ${getMonthName(currentMonth)}`);
            }
            return days;
        }

        function pricesFromLastMonth(type) {
            let randomPrices = []
            if (type) {
                for (let i = 0; i < 31; i++) {
                    randomPrices.push(Math.floor(Math.random() * 7500));
                }
            } else {
                for (let i = 0; i < 31; i++) {
                    randomPrices.push(Math.floor(Math.random() * 10000));
                }
            }
            return randomPrices;
        }

        let daysInCurrentMonth = getDaysInCurrentMonth();

        const chartData = {
            labels: daysInCurrentMonth,
            datasets: [
                {
                    label: 'This month',
                    data: pricesFromLastMonth(true),
                    fill: false,
                    borderColor: 'rgb(116, 69, 251)',
                    hoverBackgroundColor: '#FAFAFA',
                    hoverBorderColor: '#7445FB',
                    pointBackgroundColor: '#7445FB',
                    pointBorderColor: '#7445FB',
                    pointRadius: 0
                },
                {
                    label: 'Last month',
                    data: pricesFromLastMonth(false),
                    fill: false,
                    borderColor: '#858585',
                    pointBackgroundColor: '#D5D5D6',
                    pointBorderColor: '#D5D5D6',
                    borderDash: [10, 5],
                    pointRadius: 0
                },

            ],
        };

        const options = {
            responsive: true,
            maintainAspectRatio: false,
            tension: 0.4,
            plugins: {
                legend: {
                    position: 'bottom',
                    align: 'start',
                    labels: {
                        usePointStyle: true,
                        pointStyle: 'circle',
                        font: {
                            size: 12
                        },
                        display: false
                    }
                },
                tooltip: {
                    titleAlign: 'center',
                    cornerRadius: '4',
                    displayColors: false,
                    callbacks: {
                        title: function () { },
                    }
                }
            },
            hover: {
                intersect: false
            },
            scales: {
                y: {
                    grid: {
                        borderDash: [10, 5]
                    },
                },
                x: {
                    grid: {
                        display: false
                    },
                    ticks: {
                        display: false
                    }
                }
            }
        }


        return {
            chartData,
            options
        }
    }
}
</script>

<style scoped>
.crypto__summary--container {
    background: var(--light-gray-background);
    border-radius: 16px;

    display: flex;
    flex-direction: column;
    flex-wrap: wrap;
    height: 336px;
}

.crypto__summary--texts {
    display: flex;
}

.crypto__summary--title {
    margin: 32px;
    font-weight: 500;
    font-size: 20px;
}
</style>