<template>
    <div class="crypto_balance_container">
        <div class="crypto_balance_texts">
            <h3 class="crypto_balance_title">Current Balance</h3>
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

        <div class="crypto_balance_summary">
            <span class="crypto_balance_price"><span class="dollar crypto_balance_price">$</span>21 432.23</span>
            <div class="crypto_balance_summary_text">
                <img :src="arrowUpIcon" alt="arrow up icon" class="arrow-up-icon">
                <span class="arrow-up-text">12% vs last month</span>
            </div>
        </div>

        <div class="crypto_balance_buttons">
            <button class="crypto_balance_button crypto_balance_button--violet">Quick Invest</button>
            <button class="crypto_balance_button crypto_balance_button--white">Show Report</button>
        </div>

    </div>
</template>

<script>
import arrowUpIcon from "@/assets/icons/arrow_up.svg";

export default {
    name: 'CurrentBalance',
    setup() {
        return {
            arrowUpIcon
        }
    }
}
</script>

<style scoped>
.crypto_balance_container {
    background: var(--light-gray-background);
    border-radius: 16px;

    display: flex;
    flex-direction: column;
    height: 336px;
}

.crypto_balance_texts {
    display: flex;
}

.arrow-up-icon,
.arrow-up-text {
    vertical-align: middle;
    display: inline-block;
}

.crypto_balance_title {
    font-size: 20px;
    line-height: 24px;
    font-weight: 500;
    margin: 32px;
}

.crypto_balance_summary {
    margin: 32px;
}

.crypto_balance_price {
    color: var(--black-text);
    font-size: 56px;
    font-weight: 500;
}

.dollar {
    color: var(--dark-gray-text);
}

.crypto_balance_buttons {
    display: flex;
    gap: 15px;
    margin: 32px;
}

.crypto_balance_button {
    border: none;
    height: 40px;
    width: 109px;

    font-weight: 600;
    font-size: 14px;
    line-height: 24px;
    border-radius: 4px;

    transition: 0.5s ease-in-out;
    cursor: pointer;
}
</style>