<template>
    <div class="convert">
        <h1 class="convert-title">Конвертация валют</h1>
        <div class="convert-row">
            <select v-model="currency1" class="currency-select">
                <option v-for="currency in currencies" :key="currency" :value="currency">{{ currency }}</option>
            </select>
            <input type="number" v-model.number="amount1" @input="convert('input1')" class="currency-input" />
        </div>
        <div class="convert-row">
            <select v-model="currency2" class="currency-select">
                <option v-for="currency in currencies" :key="currency" :value="currency">{{ currency }}</option>
            </select>
            <input type="number" v-model.number="amount2" @input="convert('input2')" class="currency-input" />
        </div>
    </div>
</template>

<script>
import axios from 'axios';

export default {
    props: {
        currencies: Array
    },
    data() {
        return {
            currency1: 'RUB',
            currency2: 'USD',
            amount1: 1,
            amount2: 0,
            rates: {}
        };
    },
    async created() {
        await this.fetchRates();
        this.convert('input1');
    },
    methods: {
        async fetchRates() {
            const response = await axios.get('https://status.neuralgeneration.com/api/currency');
            this.rates = response.data;
        },
        convert(input) {
            const rate1 = this.getRate(this.currency1, this.currency2);
            const rate2 = this.getRate(this.currency2, this.currency1);
            if (input === 'input1') {
                this.amount2 = (this.amount1 * rate1).toFixed(2);
            } else {
                this.amount1 = (this.amount2 * rate2).toFixed(2);
            }
        },
        getRate(currency1, currency2) {
            return this.rates[`${currency1.toLowerCase()}-${currency2.toLowerCase()}`] || 1;
        }
    }
};
</script>

<style scoped>
.convert {
    padding: 20px;
    max-width: 500px;
    margin: 0 auto;
    background-color: #f9f9f9;
    border-radius: 8px;
    box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
}

.convert-title {
    text-align: center;
    font-size: 24px;
    color: #333;
    margin-bottom: 20px;
}

.convert-row {
    display: flex;
    justify-content: space-between;
    margin-bottom: 15px;
}

.currency-select {
    width: 45%;
    padding: 10px;
    font-size: 16px;
    border: 1px solid #ddd;
    border-radius: 4px;
    box-shadow: 0 1px 3px rgba(0, 0, 0, 0.1);
}

.currency-input {
    width: 45%;
    padding: 10px;
    font-size: 16px;
    border: 1px solid #ddd;
    border-radius: 4px;
    box-shadow: 0 1px 3px rgba(0, 0, 0, 0.1);
    text-align: right;
}
</style>