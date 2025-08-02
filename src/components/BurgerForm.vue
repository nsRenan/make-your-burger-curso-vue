<template>
    <div>
        <p>Componente de Mensagem</p>
        <div>
            <form id="burger-form">
                <div class="input-container">
                    <label for="name">Nome do Cliente:</label>
                    <input type="text" name="name" id="name" v-model="nameClient" placeholder="Digite o seu Nome">
                </div>
                <div class="input-container">
                    <label for="bread">Escolha o Pão:</label>
                    <select name="bread" id="bread" v-model="bread">
                        <option value="">Selecione o seu Pão</option>
                        <option value="integral">Integral</option>
                    </select>
                </div>
                <div class="input-container">
                    <label for="meats">Escolha a Carne:</label>
                    <select name="meats" id="meats" v-model="meats">
                        <option value="">Selecione o tipo de Carne</option>
                        <option value="maminha">Maminha</option>
                    </select>
                </div>
                <div id="optional-container" class="input-container">
                    <label id="optional-title" for="meats">Selecione os Opcionais:</label>
                    <div class="checkbox-container">
                        <input type="checkbox" name="optional" id="optional" v-model="optional" value="salame">
                        <span>Salame</span>
                    </div>
                    <div class="checkbox-container">
                        <input type="checkbox" name="optional" id="optional" v-model="optional" value="Alface">
                        <span>Alface</span>
                    </div>
                    <div class="checkbox-container">
                        <input type="checkbox" name="optional" id="optional" v-model="optional" value="Cebola">
                        <span>Cebola</span>
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
export default {
    name: "BurgerForm"

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