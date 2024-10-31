<template>
  <div class="pos-container">
    <div class="product-display-container">
      <div class="product-name-display">
        <div class="product-label">NOME DO PRODUTO</div>
        <div class="product-name">Panela de pressão</div>
      </div>
    </div>

    <div class="pos-content">
      <div class="product-input">
        <FloatingLabelInput
          v-model="productCode"
          label="Código"
          type="text"
          id="productCode"
        />
        
        <div class="product-input-container">
          <div class="left-section">
            <ImageUploadInput
              label="Imagem do Produto"
              id="productImage"
              placeholder="SEM IMAGEM"
            />
          </div>

          <div class="right-section">
            <FloatingLabelInput
              v-model="quantity"
              label="Quantidade"
              type="number"
              id="quantity"
            />
            <FloatingLabelInput
              v-model="value"
              label="Unitário"
              type="number"
              id="value"
              :disabled="true"
            />
            <FloatingLabelInput
              v-model="total"
              label="Total"
              type="text"
              id="total"
              :disabled="true"
            />
          </div>
        </div>
        
        <div class="actions">
          <ActionButton buttonClass="cancel" @click="cancel">CANCELAR</ActionButton>
          <ActionButton buttonClass="clear" @click="clearAll">LIMPAR TUDO</ActionButton>
          <ActionButton buttonClass="print" @click="print">IMPRIMIR</ActionButton>
          <ActionButton buttonClass="pay" @click="pay">PAGAR</ActionButton>
        </div>
      </div>

      <div class="product-list">
        <div class="itens-list">
          <table>
            <thead>
              <tr>
                <th class="item-col">Item</th>
                <th class="description-col">Descrição</th>
                <th class="quantity-col">Quantidade</th>
                <th class="price-col">Preço</th>
                <th class="total-col">Total</th>
                <th class="delete-col">Excluir</th>
              </tr>
            </thead>
          </table>
          <div class="table-body">
            <table>
              <tbody>
                <tr v-for="(item, index) in items" :key="index">
                <td class="item-col">{{ index + 1 }}</td>
                <td class="description-col">{{ item.description }}</td>
                <td class="quantity-col">{{ formatNumber(item.quantity) }}</td>
                <td class="price-col">{{ formatCurrency(item.value) }}</td>
                <td class="total-col">{{ formatCurrency(item.total) }}</td>
                <td class="delete-col">
                  <button @click="removeItem(index)">🗑️</button>
                </td>
              </tr>
              </tbody>
            </table>
          </div>
        </div>

        <div class="itens-totais">
          <div class="totals-line">
            <span class="totals-label">Sub Total ({{ items.length }} Items)</span>
            <span class="totals-value">{{ formatCurrency(grandTotal) }}</span>
          </div>
          <div class="totals-line">
            <span class="totals-label">Imposto</span>
            <span class="totals-value">{{ formatCurrency(tax) }}</span>
          </div>
          <hr class="totals-divider" />
          <div class="total-container">
            <span class="total-highlight">Total</span>
            <span class="total-highlight">{{ formatCurrency(totalWithTax) }}</span>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import FloatingLabelInput from './FloatingLabelInput.vue';
import ImageUploadInput from './ImageUploadInput.vue';
import ActionButton from './ActionButton.vue';

export default {
  name: 'POS',
  components: {
    FloatingLabelInput,
    ImageUploadInput,
    ActionButton
  },
  data() {
    return {
      productCode: '',
      quantity: 1,
      value: 0,
      total: 0,
      volume: 0,
      grandTotal: 0,
      tax: 0,
      totalWithTax: 0,
      items: []
    };
  },
  mounted() {
    this.generateSampleItems();
  },
  methods: {
    addItem() {
      const newItem = {
        description: this.productCode, // Simplificação para exemplo
        quantity: this.quantity,
        value: this.value,
        total: this.quantity * this.value
      };
      this.items.push(newItem);
      this.resetInputs();
      this.updateTotals();
    },
    resetInputs() {
      this.productCode = '';
      this.quantity = 0;
      this.value = 0;
    },
    cancel() {
      this.items = [];
      this.updateTotals();
    },
    clearAll() {
      this.items = [];
      this.resetInputs();
      this.updateTotals();
    },
    print() {
      // Implementar lógica de impressão
      console.log('Imprimir');
    },
    pay() {
      // Implementar lógica de pagamento
      console.log('Pagar', this.items);
    },
    removeItem(index) {
      this.items.splice(index, 1);
      this.updateTotals();
    },
    updateTotals() {
      this.volume = this.items.length;
      this.grandTotal = this.items.reduce((acc, item) => acc + item.total, 0);
      this.tax = this.grandTotal * 0.1; // Exemplo de cálculo de imposto
      this.totalWithTax = this.grandTotal + this.tax;
    },
    formatNumber(value) {
      return value.toLocaleString('pt-BR');
    },
    formatCurrency(value) {
      return value.toLocaleString('pt-BR', { style: 'currency', currency: 'BRL' });
    },
    generateSampleItems() {
      for (let i = 0; i < 10; i++) {
        this.items.push({
          description: `Item ${i + 1}`,
          quantity: Math.floor(Math.random() * 10) + 1,
          value: Math.floor(Math.random() * 100) + 1,
          total: Math.floor(Math.random() * 1000) + 1
        });
      }
      this.updateTotals();
    }
  },
  watch: {
    quantity(val) {
      this.total = val * this.value;
    },
    value(val) {
      this.total = val * this.quantity;
    }
  }
};
</script>

<style scoped>
.pos-container {
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 10px 100px 100px; /* Ajuste o padding top para 60px para compensar a navbar fixa */
}

.product-display-container {
  display: flex;
  justify-content: space-between;
  width: 100%;
  text-align: center;
}

.product-name-display {
  width: 100%;
  padding: 20px;
  font-size: 24px;
  font-weight: bold;
  border: 3px solid #3366cc;
  border-radius: 10px;
  background-color: #f0f0f0;
  color: #3366cc;
  position: relative;
  box-sizing: border-box;
}

.product-label {
  font-size: 14px;
  color: #666666;
  position: absolute;
  top: 10px;
  left: 20px;
}

.product-name {
  margin-top: 20px;
  color: #3366cc;
}

.pos-content {
  display: flex;
  justify-content: space-between;
  width: 100%;
  padding: 20px;
}

.product-input {
  display: flex;
  flex-direction: column;
  width: 48%;
  background-color: #3366cc;
  padding: 20px;
  border-radius: 10px;
  margin-right: 10px;
}

.product-input-container {
  display: flex;
  justify-content: space-between;
  width: 100%;
  height: 100%; /* Ocupa a altura total */
  flex-wrap: wrap; /* Para responsividade */
}

.left-section, .right-section {
  width: 48%;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
}

.right-section {
  height: auto; /* Para ajustar a altura automaticamente */
}

.input-group {
  display: flex;
  flex-direction: column;
  margin-bottom: 10px;
  width: 100%;
}

.input-group label {
  margin-bottom: 5px;
  color: #666666;
}

.input-group input {
  padding: 10px;
  font-size: 16px;
  border: 2px solid #3366cc;
  border-radius: 5px;
}

.input-group.image-group {
  display: flex;
  justify-content: center;
  align-items: center;
  border: 2px solid #3366cc;
  border-radius: 5px;
  height: 150px;
  background-color: #fff;
}

.image-placeholder {
  color: #666666;
  font-size: 14px;
}

.actions {
  display: flex;
  justify-content: space-between; /* Alinha os botões dentro do container */
  width: 100%;
  background-color: white; /* Define o fundo branco */
  padding: 20px;
  border-radius: 10px;
  margin-top: 20px; /* Espaçamento superior de 20px */
  box-sizing: border-box; /* Para incluir padding na largura */
  flex-wrap: wrap; /* Para garantir que os botões se ajustem em telas menores */
}

.itens-totais {
  width: 100%;
  background-color: white;
  padding: 20px;
  border-radius: 5px;
  box-sizing: border-box;
  margin-top: 20px;
}

.totals-line {
  display: flex;
  justify-content: space-between;
  margin-bottom: 10px;
}

.totals-label {
  font-size: 16px;
  color: #666666;
}

.totals-left, .totals-right {
  display: flex;
  flex-direction: column;
}

.totals-left {
  align-items: flex-start;
}

.totals-right {
  align-items: flex-end;
}

.totals-title {
  font-size: 16px;
  font-weight: bold;
  margin-bottom: 20px;
}

.totals-value {
  font-size: 16px;
  color: #3366cc;
}

.total-highlight {
  font-size: 18px;
  font-weight: bold;
}

.actions button {
  padding: 10px;
  font-size: 16px;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  flex: 1; /* Para garantir que os botões cresçam igualmente */
  margin: 5px; /* Espaçamento entre os botões */
  max-width: calc(25% - 10px); /* Limite máximo de largura dos botões */
  white-space: normal; /* Permite quebra de linha dentro dos botões */
  text-align: center; /* Centraliza o texto dentro dos botões */
}

.actions button:nth-child(1) {
  background-color: #ff4d4d;
  color: #fff;
}

.actions button:nth-child(2) {
  background-color: #ffc107;
  color: #fff;
}

.actions button:nth-child(3) {
  background-color: #8e44ad;
  color: #fff;
}

.actions button:nth-child(4) {
  background-color: #28a745;
  color: #fff;
}

.product-list {
  display: flex;
  flex-direction: column;
  width: 48%;
  background-color: #3366cc;
  padding: 20px;
  border-radius: 10px;
  margin-right: 10px;
}

.itens-list {
  width: 100%;
  background-color: white;
  padding: 20px;
  border-radius: 5px;
  box-sizing: border-box;
  flex-grow: 1;
  overflow-y: auto; /* Adiciona rolagem se necessário */
  max-height: 300px; /* Altura máxima para a lista de itens */
  overflow: hidden; /* Remove a barra de rolagem da div */
}

.table-body {
  max-height: 300px; /* Define a altura máxima do corpo da tabela */
  overflow-y: auto; /* Adiciona barra de rolagem vertical */
}

.itens-list table {
  width: 100%;
  border-collapse: collapse;
}

.itens-list th, .itens-list td {
  padding: 10px;
  text-align: left;
  border-bottom: 1px solid #ddd;
}

.itens-list th {
  background-color: #3366cc;
  color: white;
  position: sticky;
  top: 0;
  z-index: 1; /* Garante que o cabeçalho fique acima das linhas da tabela */
}

.item-col {
  width: 10%;
}

.description-col {
  width: 40%;
}

.quantity-col {
  width: 10%;
}

.price-col {
  width: 15%;
}

.total-col {
  width: 20%;
}

.delete-col {
  width: 2%;
}

.itens-list td button {
  background-color: transparent;
  border: none;
  cursor: pointer;
  color: #ff4d4d;
  font-size: 18px;
}

.totals {
  display: flex;
  justify-content: space-between;
}

.volume, .grand-total {
  display: flex;
  flex-direction: column;
  align-items: flex-start;
}

.volume span, .grand-total span {
  font-size: 14px;
  color: #666666;
}

.volume input, .grand-total input {
  padding: 10px;
  font-size: 16px;
  border: 2px solid #3366cc;
  border-radius: 5px;
  width: 100%;
}

.totals-divider {
  color: #666666;  
}

.text-right {
  text-align: right;
}

.total-container {
  display: flex;
  justify-content: space-between;
  padding: 10px 0;
  background-color: #3366cc;
  color: white;
  padding: 20px;
  border-radius: 10px;
}
</style>
