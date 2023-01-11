<template>
    <div>
        <Message :msg="msg" v-show="msg"/>
        <div>
            <form id="burger-form" @submit="createBurger">
                <div class="input-container">
                    <label for="nome">Nome do cliente:</label>
                    <input 
                        type="text" 
                        id="nome" 
                        name="nome"
                        v-model="nome"
                        placeholder="Digite seu nome"
                    />
                </div>
                <div class="input-container">
                    <label for="pao">Escolha o pão:</label>
                    <select name="pao" id="pao" v-model="pao">
                        <option value="">Escolha seu pão</option>
                        <option
                            v-for="pao in paes"
                            :key="pao.id"
                            :value="pao.tipo"
                            >{{ pao.tipo }}
                        </option>
                    </select>
                </div>
                <div class="input-container">
                    <label for="carne">Escolha a carne do seu burger:</label>
                    <select name="carne" id="carne" v-model="carne">
                        <option value="">Selecione o corte da carne</option>
                        <option
                            v-for="carne in carnes"
                            :key="carne.id"
                            :value="carne.tipo"
                            >{{ carne.tipo }}
                        </option>
                    </select>
                </div>
                <div id="opcionais-container" class="input-container">
                    <label id="opcionais-title" for="opcionais">Selecione os opicionais:</label>
                    <div 
                        class="checkbox-container" 
                        v-for="opcional in opcionaisdata"
                        :key="opcional.id"
                    >
                        <input 
                            type="checkbox" 
                            name="opcionais" 
                            :value="opcional.tipo" 
                            v-model="opcionais" />
                        <span>{{ opcional.tipo }}</span>
                    </div>
                </div>
                <div class="input-container">
                    <input type="submit" class="Submit-btn" value="Criar meu Burger!"/>
                </div>
            </form>
        </div>
    </div>
</template>

<script>
    import Message from './Message.vue';

    export default {
        name: 'BurgerForm',
        components: {
            Message,
        },
        data() {
            return {
                paes: null,
                carnes: null,
                opcionaisdata: null,
                nome: null,
                pao: null,
                carne: null,
                opcionais: [],
                msg: null,
            }
        },
        methods: {
            async getIngredientes() {
                const req = await fetch('http://localhost:3001/ingredientes');
                const data = await req.json(); 
                this.paes = data.paes;
                this.carnes = data.carnes;
                this.opcionaisdata = data.opcionais;
            },
            async createBurger(e) {
                e.preventDefault();
                const data = {
                    nome: this.nome,
                    pao: this.pao,
                    carne: this.carne,
                    opcionais: Array.from(this.opcionais),
                    status: "Solicitado",
                }
                const datajason = JSON.stringify(data);
                const req = await fetch('http://localhost:3001/burgers', {
                    method: 'POST',
                    headers: {
                        'Content-Type': 'application/json'
                    },
                    body: datajason
                });
                const res = await req.json();

                this.msg = `Pedido Nº ${res.id} realizado com sucesso!`
                setTimeout(() => this.msg = "", 3000);

                this.nome = "";
                this.carne = "";
                this.pao = "";
                this.opcionais = "";
            }
        },
        mounted() {
            this.getIngredientes();
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
        margin-bottom: 15px;
        font-weight: bold;
        color: #222;
        padding: 5px 10px;
        border-left: 4px solid #fcba03;
    }
    input, select {
        padding: 5px 10px;
        width: 300px;
    }
    #opcionais-container {
        flex-direction: row;
        flex-wrap: wrap;
    }
    #opcionais-title {
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
    .Submit-btn {
        background-color: #222;
        color: #fcba03;
        font-weight: bold;
        border: 2px solid #222;
        padding: 10px;
        font-size: 16px;
        width: 100%;
        margin: 0 auto;
        cursor: pointer;
        transition: .5s;
        border-radius: 5px;
    }
    .Submit-btn:hover {
        background-color: transparent;
        color: #222;
    }
</style>