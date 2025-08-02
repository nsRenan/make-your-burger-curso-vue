<template>
    <div>
        <Message :message="message" v-show="message"/>
        <div>
            <form id="burger-form" @submit="createBurger">
                <div class="input-container">
                    <label for="name">Nome do Cliente:</label>
                    <input type="text" name="name" id="name" v-model="name" placeholder="Digite o seu Nome">
                </div>
                <div class="input-container">
                    <label for="bread">Escolha o Pão:</label>
                    <select name="bread" id="bread" v-model="bread">
                        <option value="">Selecione o seu Pão</option>
                        <option v-for="bread in breads" :key="bread.id" :value="bread.tipo">{{ bread.tipo }}</option>
                    </select>
                </div>
                <div class="input-container">
                    <label for="meats">Escolha a Carne:</label>
                    <select name="meats" id="meats" v-model="meat">
                        <option value="">Selecione o tipo de Carne</option>
                        <option v-for="meat in meats" :key="meat.id" :value="meat.tipo">{{ meat.tipo }}</option>
                    </select>
                </div>
                <div id="optional-container" class="input-container">
                    <label id="optional-title" for="meats">Selecione os Opcionais:</label>
                    <div v-for="option in optionalData" :key="option.id" class="checkbox-container">
                        <input type="checkbox" name="optional" id="optional" v-model="optional" :value="option.tipo">
                        <span>{{ option.tipo }}</span>
                    </div>
                </div>
                <div class="input-container">
                    <input type="submit" class="submit-btn" value="Criar meu Burger">
                </div>
            </form>
        </div>
    </div>

</template>

<script>
import Message from './Message.vue'
export default {
    name: "BurgerForm",
    data() {
        return {
            breads: null,
            meats: null,
            optionalData: null,
            name: null,
            bread: null,
            meat: null,
            optional: [],
            message: null,
        }
    },
    methods: {
        async getIngredients() {
            const req = await fetch('http://localhost:3000/ingredientes');
            const data = await req.json();

            this.breads = data.paes;
            this.meats = data.carnes;
            this.optionalData = data.opcionais;

        },
        async createBurger(e) {
            e.preventDefault();

            const data = {
                name: this.name,
                meat: this.meat,
                bread: this.bread,
                optional: Array.from(this.optional),
                status: "Solicitado"

            }
            const dataJson = JSON.stringify(data)

            const req = await fetch("http://localhost:3000/burgers", {
                method: "POST",
                headers: {
                    "Content-Type": "application/json"
                },
                body: dataJson
            });

            const res = await req.json();

            this.message = `Pedido Nº ${res.id} realizado com sucesso`
            setTimeout(() => this.message = "", 3000);

            this.name = '';
            this.meat = '';
            this.bread = '';
            this.optional = '';


        }
    },
    mounted() {
        this.getIngredients()
    },

    components: {
        Message
    }


}
</script>

<style scoped>
#burger-form {
    max-width: 400px;
    margin: 0 auto;
}

.input-container {
    display: flex;
    flex-direction: column;
    margin-bottom: 20px;
}

label {
    font-weight: bold;
    margin-bottom: 15px;
    padding: 5px 10px;
    color: #222;
    border-left: 4px solid #FCBA03;
}

input,
select {
    padding: 5px 10px;
    width: 300px;
}

#optional-container {
    flex-direction: row;
    flex-wrap: wrap;
}

#optional-title {
    width: 100%;
}

.checkbox-container {
    display: flex;
    align-items: flex-start;
    width: 50%;
    margin-bottom: 20px;
}

.checkbox-container span,
.checkbox-container input {
    width: auto;
}

.checkbox-container span {
    margin-left: 6px;
    font-weight: bold;
}

.submit-btn {
    font-weight: bold;
    font-size: 16px;
    padding: 10px;
    margin: 0 auto;
    cursor: pointer;
    transition: 0.5s;
    border: 2px solid #222;
    background-color: #222;
    color: #FCBA03;
}

.submit-btn:hover {
    background-color: transparent;
    color: #222;
}
</style>