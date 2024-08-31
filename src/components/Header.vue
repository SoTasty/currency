<template>
    <header class="header">
        <nav class="nav">
            <router-link to="/" class="nav-link">Главная</router-link>
            <router-link to="/convert" class="nav-link">Конвертация</router-link>
            <select v-model="baseCurrency" @change="updateBaseCurrency" class="currency-select">
                <option v-for="currency in currencies" :key="currency" :value="currency">{{ currency }}</option>
            </select>
        </nav>
    </header>
</template>

<script>
export default {
    props: {
        baseCurrency: String,
        currencies: Array
    },
    data() {
        return {
            baseCurrency: 'RUB'
        };
    },
    methods: {
        updateBaseCurrency() {
            this.$emit('currency-changed', this.baseCurrency);
        }
    }
};
</script>

<style scoped>
.header {
    background-color: #007bff;
    padding: 10px 20px;
    display: flex;
    justify-content: space-between;
    align-items: center;
    color: #fff;
    border-top-left-radius: 8px;
    border-top-right-radius: 8px;
    box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
}

.nav {
    display: flex;
    align-items: center;
}

.nav-link {
    color: #fff;
    margin-right: 20px;
    text-decoration: none;
    font-weight: bold;
    transition: color 0.3s ease;
}

.nav-link:hover {
    color: #ffd700;
}

.currency-select {
    padding: 5px;
    font-size: 16px;
    border-radius: 4px;
    border: 1px solid #ddd;
}
</style>