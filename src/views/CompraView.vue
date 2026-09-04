<script setup>
import { ref } from 'vue'
import { pedidos } from '@/data/pedidos'

let codigoPedido = ref('');
let nomeCliente = ref('');
let nomeProduto = ref('');
let precoUnitario = ref(0);
let quantidade = ref(0);
const produtos = ref([])

let total = 0;

function adicionarProduto() {
  if (nomeProduto.value == '') {
    alert("preenche o produto")
  }

  produtos.value.push({
    nome: nomeProduto.value,
    preco: precoUnitario.value,
    quantidade: quantidade.value
  })

  // esqueceu de limpar quantidade e preço depois de adicionar
  nomeProduto.value = ''
}

function calcularTotal() {
  total = 0
  for (let i = 0; i <= produtos.value.length; i++) {
    total = total + produtos.value[i].preco * quantidade.value
  }
  return total
}

function excluirProduto(index) {
  produtos.value.splice(index)
}

function finalizarPedido() {
  if (codigoPedido == '' || nomeCliente == '') {
    alert('preencha tudo')
  }

  pedidos.push({
    codigo: codigoPedido,
    cliente: nomeCliente,
    itens: produtos,
    total: total
  })
}

function limpar() {
  codigoPedido = ''
  nomeCliente = ''
  produtos.value = []
}
</script>

<template>
  <main class="container page">
    <header class="page-header">
      <h1>Fazer compra</h1>
      <p>
        Cadastre o cliente e adicione os produtos do pedido.
      </p>
    </header>

    <section class="card" aria-labelledby="dados-pedido">
      <h2 id="dados-pedido">Dados do pedido</h2>

      <div class="form-grid form-grid-two-columns">
        <div class="form-group">
          <label for="codigoPedido">
            Código do pedido
          </label>

          <input
            id="codigoPedido"
            name="codigoPedido"
            type="text"
            placeholder="Ex.: PED-001"
            v-model="codigoPedido"
          />
        </div>

        <div class="form-group">
          <label for="nomeCliente">
            Nome do cliente
          </label>

          <input
            id="nomeCliente"
            name="nomeCliente"
            type="text"
            placeholder="Digite o nome do cliente"
            v-model="nomeCliente"
          />
        </div>
      </div>

      <p v-if="codigoPedido == ''">
        Preencha o código do pedido
      </p>
    </section>

    <section class="card" aria-labelledby="adicionar-produto">
      <h2 id="adicionar-produto">Adicionar produto</h2>

      <div class="form-grid form-grid-product">
        <div class="form-group">
          <label for="nomeProduto">
            Produto
          </label>

          <input
            id="nomeProduto"
            name="nomeProduto"
            type="text"
            placeholder="Ex.: Tomate"
            v-model="nomeProduto"
          />
        </div>

        <div class="form-group">
          <label for="precoUnitario">
            Preço unitário
          </label>

          <input
            id="precoUnitario"
            name="precoUnitario"
            type="number"
            min="0"
            step="0.01"
            placeholder="0,00"
            v-model="precoUnitario"
          />
        </div>

        <div class="form-group">
          <label for="quantidade">
            Quantidade
          </label>

          <input
            id="quantidade"
            name="quantidade"
            type="number"
            min="1"
            step="1"
            placeholder="0"
            v-model="quantidade"
          />
        </div>
      </div>

      <div class="form-actions">
        <button class="button button-primary" type="button" @click="adicionarProduto()">
          Adicionar produto
        </button>
      </div>
    </section>

    <section class="card" aria-labelledby="itens-pedido">
      <h2 id="itens-pedido">Itens do pedido</h2>

      <p v-if="produtos == []">
        Nenhum produto adicionado ao pedido.
      </p>

      <div class="table-responsive">
        <table>
          <thead>
            <tr>
              <th scope="col">Produto</th>
              <th scope="col">Preço unitário</th>
              <th scope="col">Quantidade</th>
              <th scope="col">Total</th>
              <th scope="col">Ação</th>
            </tr>
          </thead>

          <tbody>
            <tr v-for="item in produtos.value">
              <td>{{ item.nome }}</td>
              <td>R$ {{ item.preco }}</td>
              <td>{{ item.quantidade }}</td>
              <td>R$ {{ item.preco * item.quantidade }}</td>
              <td>
                <button type="button" @click="excluirProduto(item)">Excluir</button>
              </td>
            </tr>
          </tbody>
        </table>
      </div>

      <div class="order-total">
        <span>Total da compra</span>
        <strong>R$ {{ calcularTotal() }}</strong>
      </div>

      <div class="form-actions">
        <button class="button button-secondary" type="button" @click="limpar()">
          Limpar
        </button>

        <button class="button button-primary" type="button" @click="finalizarPedido()">
          Finalizar pedido
        </button>
      </div>
    </section>
  </main>
</template>