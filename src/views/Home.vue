<template>
    <div class="home">
        <h1 class="home-title">Курсы валют</h1>
        <ul class="currency-list">
            <li v-for="(rate, currency) in rates" :key="currency" class="currency-item">
                <span class="currency-name">1 {{ baseCurrency.toUpperCase() }}</span>
                <span class="currency-rate">= {{ rate.toFixed(2) }} {{ currency.split('-')[1].toUpperCase() }}</span>
            </li>
        </ul>
    </div>
</template>

<script>
import axios from 'axios';

export default {
    props: {
        baseCurrency: String,
        currencies: Array
    },
    data() {
        return {
            rates: {}
        };
    },
    async created() {
        await this.fetchRates();
    },
    methods: {
        async fetchRates() {
            const response = await axios.get('https://status.neuralgeneration.com/api/currency');
            const data = response.data;
            const filteredData = Object.fromEntries(
                Object.entries(data).filter(([key]) => key.startsWith(`${this.baseCurrency.toLowerCase()}-`) && this.currencies.includes(key.split('-')[1].toUpperCase()))
            );
            this.rates = filteredData
        }
    },
    watch: {
        baseCurrency: 'fetchRates'
    }
};
</script>

<style scoped>
.home {
    padding: 20px;
    max-width: 600px;
    margin: 0 auto;
    background-color: #f9f9f9;
    border-radius: 8px;
    box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
}

.home-title {
    text-align: center;
    font-size: 24px;
    color: #333;
    margin-bottom: 20px;
}

.currency-list {
    list-style: none;
    padding: 0;
}

.currency-item {
    display: flex;
    justify-content: space-between;
    padding: 10px 15px;
    background-color: #ffffff;
    border: 1px solid #ddd;
    border-radius: 4px;
    margin-bottom: 10px;
    transition: background-color 0.3s ease;
}

.currency-item:hover {
    background-color: #f1f1f1;
}

.currency-name {
    font-weight: bold;
    color: #555;
}

.currency-rate {
    color: #007bff;
}
</style>