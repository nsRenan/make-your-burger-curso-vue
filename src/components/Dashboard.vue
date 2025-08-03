<template>
    <div id="burger-table">
        <Message :message="message" v-show="message" />
        <div>
            <div id="burger-table-heading">
                <div class="order-id">#:</div>
                <div>cliente:</div>
                <div>Pão:</div>
                <div>Carne:</div>
                <div>Opcionais:</div>
                <div>Ações:</div>
            </div>
        </div>
        <div id="burger-table-rows">
            <div class="burger-table-row" v-for="burger in burgers" :key="burger.id">
                <div class="order-number">{{ burger.id }}</div>
                <div>{{ burger.name }}</div>
                <div>{{ burger.bread }}</div>
                <div>{{ burger.meat }}</div>
                <div>
                    <ul>
                        <li v-for="(optional, index) in burger.optional" :key="index">{{ optional }}</li>
                    </ul>
                </div>
                <div>
                    <select name="status" classe="status" @change="updateBurger($event, burger.id)">
                        <option v-for="state in status" :key="state.id" :value="state.tipo"
                            :selected="burger.status == state.tipo">{{ state.tipo }}</option>
                    </select>
                    <button class="delete-btn" @click="deleteBurger(burger.id)">Cancelar</button>
                </div>
            </div>
        </div>
    </div>

</template>

<script>
import Message from './Message.vue';
export default {
    name: "Dashboard",
    data() {
        return {
            burgers: null,
            burgers_id: null,
            status: [],
            message: null
        }
    },
    components: {
        Message
    },
    methods: {
        async getPedidos() {
            const req = await fetch("http://localhost:3000/burgers");

            const data = await req.json();
            this.burgers = data;

            this.getStatus();

        },
        async getStatus() {
            const req = await fetch("http://localhost:3000/status");

            const data = await req.json();
            this.status = data
        },
        async deleteBurger(id) {
            const req = await fetch(`http://localhost:3000/burgers/${id}`, {
                method: "DELETE"
            });
            const res = await req.json();

            this.message = `Pedido Nº ${res.id} de ${res.name} cancelado com sucesso`
            setTimeout(() => this.message = "", 5000);

            this.getPedidos();
        },
        async updateBurger(event, id) {

            const option = event.target.value

            const dataJson = JSON.stringify({ status: option });

            const req = await fetch(`http://localhost:3000/burgers/${id}`, {
                method: "PATCH",
                headers: { "Content-Type": "application/json" },
                body: dataJson
            });

            const res = await req.json();
            
            this.message = `Pedido Nº ${res.id} de ${res.name} atualizado para ${res.status}`
            setTimeout(() => this.message = "", 5000);

        }

    },
    mounted() {
        this.getPedidos();
    }
}
</script>

<style scoped>
#burger-table {
    max-width: 1200px;
    margin: 0 auto;
}

#burger-table-heading,
#burger-table-rows,
.burger-table-row {
    display: flex;
    flex-wrap: wrap;
}

#burger-table-heading {
    font-weight: bold;
    padding: 12px;
    border-bottom: 3px solid #333;
}

#burger-table-heading div,
.burger-table-row div {
    width: 19%;
}

.burger-table-row {
    width: 100%;
    padding: 12px;
    border-bottom: 1px solid #CCC;
}

#burger-table-heading .order-id,
.burger-table-row .order-number {
    width: 5%;
}

select {
    padding: 12px 6px;
    margin-right: 12px;
}

.delete-btn {
    background-color: #222;
    color: #FCBA03;
    font-weight: bold;
    border: 2px solid #222;
    padding: 10px;
    font-size: 16px;
    margin: 0 auto;
    cursor: pointer;
    transition: 0.5s;
}

.delete-btn:hover {
    background-color: transparent;
    color: #222;
}
</style>