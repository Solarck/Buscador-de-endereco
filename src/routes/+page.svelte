<script lang="ts">
    import type DAdosApi from "../utils/DadosApi";
    import { buscaCep } from "../utils/requisicaoApi";
    import montaCep from "../utils/dadosDoCep";

    let input: any;
    function mascaraDeCep() {
        let inputLength = input.value.length;
        if (inputLength == 5) {
            input.value += "-";
        }
    }

    let cepValue: any;
    let statusDeErro: boolean | null = null;
    let dadosApi: DAdosApi | null = null;

    async function aoSubmeter() {
        const conexaoApi = await buscaCep(cepValue);
        const conexaoConvertida = await conexaoApi.json();

        dadosApi = montaCep(conexaoConvertida);

        if (conexaoConvertida.erro) {
            dadosApi = null;
            statusDeErro = true;
        } else {
            statusDeErro = false;
            dadosApi;
        }
    }
</script>

<header class="main">
    <section class="container">
        <h1 class="legenda">
            Buscamos qualquer endereço, do Brasil, para você.
        </h1>

        <form class="input">
            <input
                class="input-num"
                maxlength="9"
                autocomplete="off"
                required
                bind:this={input}
                on:keypress={mascaraDeCep}
                bind:value={cepValue}
                class:erro-input={statusDeErro === true}
            />

            {#if statusDeErro === true}
                <span><p class="aviso">O cep não existe!</p></span>
            {/if}

            <div class="botao">
                <button class="botao-busca" on:click={aoSubmeter}>Buscar</button>
            </div>
        </form>
    </section>

    {#if dadosApi}
        <section class="container-section">
            <div class="campo-de-endereco">
                <h1 class="titulo">CEP: {dadosApi.cep}</h1>
                <h1 class="titulo">Logradouro: {dadosApi.logradouro}</h1>
                <h1 class="titulo">Bairro: {dadosApi.bairro}</h1>
                <h1 class="titulo">Localidade: {dadosApi.localidade}</h1>
                <h1 class="titulo">UF: {dadosApi.uf}</h1>
            </div>
        </section>
    {/if}
</header>

<style>
    @import url("https://fonts.googleapis.com/css2?family=Golos+Text:wght@500&display=swap");
    :global(body) {
        font-family: "Golos Text", sans-serif;
        background: rgba(11, 11, 11, 0.466);
        inset: 0;
    }

    .main {
        display: flex;
        justify-content: center;
    }

    .container {
        margin-top: 100px;
        width: 450px;
        height: 450px;
        background: #ffffffe7;
        display: inline-block;
        box-shadow: 0px 4px 5px 2px rgba(0, 0, 0, 5);
        border-radius: 15px;
    }

    .legenda {
        margin-top: 50px;
        text-align: center;
    }

    .input {
        flex-direction: column;
        display: flex;
        justify-content: center;
        align-items: center;
    }

    .input-num {
        margin-bottom: 20px;
        margin-top: 20px;
        width: 250px;
        height: 50px;
        border-radius: 10px;
        padding-left: 15px;
        font-size: 18px;
        font-family: "Golos Text", sans-serif;
        font-weight: 300;
    }

    .aviso {
        display: inline-block;
        justify-content: center;
        margin: 15px;
        color: red;
    }

    .erro-input {
        border: 1px solid #ff003e;
        margin-bottom: 0px;
    }

    .botao {
        display: flex;
        justify-content: center;
    }

    .botao-busca {
        border: none;
        background: rgb(240, 11, 99);
        font-size: 18px;
        color: #ffff;
        width: 170px;
        height: 50px;
        border-radius: 10px;
        font-family: "Golos Text", sans-serif;
        font-weight: 300;
    }

    .botao-busca:hover {
        cursor: pointer;
        background: rgb(240, 90, 147);
    }

    .container-section {
        padding: 16.5px;
        margin-left: 20px;
        margin-top: 100px;
        width: 450px;
        height: 410px;
        background: #ffffffe7;
        display: block;
        justify-content: center;
        flex-direction: column;
        box-shadow: 0px 4px 5px 2px rgba(0, 0, 0, 5);
        border-radius: 15px;
    }

    .titulo {
        justify-content: start;
    }

    @media screen and (max-width: 750px) {
        .main {
            display: flex;
            justify-content: center;
            align-items: center;
            flex-direction: column;
        }

        .container {
            width: 97%;
            margin-top: 50px;
            margin-bottom: 18px;
        }

        .container-section {
            width: 88%;
            margin-left: 0;
            margin-top: 0;
            margin-bottom: 50px;
        }

        .legenda {
            padding: 10px;
        }
    }
</style>
