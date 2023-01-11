<template>
    <div id="burger-table">
        <div>
            <div id="burger-table-heading">
                <div class="order-id">#:</div>
                <div>Cliente</div>
                <div>Pão:</div>
                <div>Carne:</div>
                <div>Opicionais:</div>
                <div>Ações</div>
            </div>
        </div>
        <div id="burger-table-rows">
            <div 
                class="burger-table-row"
                v-for="burger in burgers"
                :key="burger.id"
            >
                <div class="order-number">{{ burger.id }}</div>
                <div>{{ burger.nome }}</div>
                <div>{{ burger.pao }}</div>
                <div>{{ burger.carne }}</div>
                <div>
                    <ul>
                        <li
                            v-for="(opcional, index) in burger.opcionais"
                            :key="index"
                        >{{ opcional }}</li>
                    </ul>
                </div>
                <div>
                    <select name="status" class="status">
                        <option value="">Selecione</option>
                        <option 
                            value="s.tipo"
                            v-for="s in status"
                            :key="s.id"
                            :selected="s.tipo === burger.status"
                        >{{ s.tipo }}
                        </option>
                    </select>
                    <button class="delete-btn">Cancelar</button>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
    export default {
        name: 'Dashboard',
        data() {
            return {
                burgers: null,
                burger_id: null,
                status: [],
            }
        },
        methods: {
            async getPedidos() {
                const req = await fetch("http://localhost:3001/burgers");
                const data = await req.json();

                this.burgers = data;
                console.log(this.burgers);

                this.getStatus();
            },
            async getStatus() {
                const req = await fetch("http://localhost:3001/status");
                const data = await req.json();

                this.status = data;
            }
        },
        mounted() {
            this.getPedidos();
        },
    }
</script>

<style>
    #burger-table {
        max-width: 1200px;
        margin: 0 auto;
    }
    #burger-table-heading,
    #burger-table-rows,
    .burger-table-row {
        display: flex;
        flex-wrap: wrap ;
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
        border-bottom: 1px solid #ccc;
    }
    #burger-table-heading .order-id,
    .burger-table-row .order-number {
        width: 5%;
    }
    select {
        padding: 12px 6px;
        margin-right: 12px;
        border-radius: 5px;
    }
    .delete-btn {
        background-color: #222;
        color: #fcba03;
        font-weight: bold;
        padding: 10px;
        font-size: 16px;
        border: 2px solid #222;
        cursor: pointer;
        margin: 0 auto;
        transition: .5s;
        border-radius:  5px;
    }
    .delete-btn:hover {
        background-color: transparent;
        color: #222;
    }
</style>