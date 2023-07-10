<template>
    <div>
        <p>Componente de mensagem</p>
        <div>
            <form id="pizza-form" @submit="createPizza">
                <div class="input-container">
                    <label for="nome">Nome do cliente</label>
                    <input type="text" id="nome" name="nome" v-model="nome" placeholder="Digite o seu nome">
                </div>
                <div class="input-container">
                    <label for="massa">Escolha o tipo de massa: </label>
                    <select name="massa" id="massa" v-model="massa">
                        <option value="">Selecione a sua massa</option>
                        <option v-for="massa in massas" :key="massa.id" :value="massa.tipo">
                            {{ massa.tipo }}
                        </option>
                    </select>
                </div>
                <div class="input-container">
                    <label for="recheio">Escolha o recheio da sua pizza: </label>
                    <select name="recheio" id="recheio" v-model="recheio">
                        <option value="">Selecione o seu recheio</option>
                        <option v-for="recheio in recheios" :key="recheio.id" :value="recheio.tipo">
                            {{ recheio.tipo }}
                        </option>
                    </select>
                </div>
                <div id="opcionais-container" class="input-container">
                    <label id="opcionais-title" for="opcionais">Selecione os opcionais: </label>
                    <div class="checkbox-container" v-for="opcional in opcionaisdata" :key="opcional.id">
                        <input type="checkbox" name="opcionais" v-model="opcionais" :value="opcional.tipo">
                        <span>
                            {{ opcional.tipo }}
                        </span>
                    </div>
                </div>
                <div class="input-container">
                    <input type="submit" class="submit-btn" value="Criar minha pizza!">
                </div>
            </form>
        </div>
    </div>    
</template>

<script>
export default {
    name: "PizzaForm",
    data() {
        return {
            massas: null,
            recheios: null,
            opcionaisdata: null,
            nome: null,
            massa: null,
            recheio: null,
            opcionais: [],
            msg: null
        }
    },
    methods: {
        async getIngredientes() {

            const req = await fetch('http://localhost:3000/ingredientes');
            const data = await req.json();

            this.massas = data.massas;
            this.recheios = data.recheios;
            this.opcionaisdata = data.opcionais;

        },
        async createPizza(e) {
            e.preventDefault();

            const data = {
                nome: this.nome,
                recheio: this.recheio,
                massa: this.massa,
                opcionais: Array.from(this.opcionais),
                status: "Solicitado"
            }

            const dataJson = JSON.stringify(data);

            const req = await fetch("http://localhost:3000/pizzas", {
                method: "POST",
                headers: { "Content-Type": "application/json" },
                body: dataJson
            });

            const res = await req.json();
            
            // Adicionar mensagem de sistema

            // limpar msg

            //Limpar os campos
            this.nome = "";
            this.recheio = "";
            this.massa = "";
            this.adicionais = "";

        }
    },

    mounted() {
        this.getIngredientes();
    }
}

</script>

<style scoped>
    #pizza-form {
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
        color: #222;
        padding: 5px 10px;
        border-left: 4px solid #FCBA03;
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

    .submit-btn {
        background-color: #222;
        color:#FCBA03;
        font-weight: bold;
        border: 2px solid #222;
        padding: 10px;
        font-size: 16px;
        margin: 0 auto;
        cursor: pointer;
        transition: .5s;
    }
    .submit-btn:hover {
        background-color: transparent;
        color: #222;
    }

</style>