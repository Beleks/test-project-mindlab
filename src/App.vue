<script setup>
import MainTable from "@/components/MainTable.vue";

import { ref } from "vue";

// fieldType: 'select', 'input', 'formula'

const columns = [
  {
    // NOTE: Нужен ли был title?
    title: "Статус",
    objKey: "status",
    isReadOnly: false,
    fieldType: "select",
  },
  {
    title: "Заказ",
    objKey: "orderName",
    isReadOnly: true,
    fieldType: "input",
  },
  {
    title: "Цена за шт.",
    objKey: "price", // Цена за шт.
    isReadOnly: true,
    fieldType: "input",
  },
  {
    title: "Всего",
    objKey: "qty", // Всего
    isReadOnly: true,
    fieldType: "input",
  },
  {
    title: "Кол-во отгруженного",
    objKey: "qtyShipped", // Кол-во отгруженного
    isReadOnly: false,
    fieldType: "input",
  },
  {
    title: "Кол-во оплаченного",
    objKey: "qtyPaid", // Кол-во оплаченного
    isReadOnly: false,
    fieldType: "input",
  },
  {
    title: "К оплате",
    objKey: "forPayment",
    isReadOnly: true,
    fieldType: "formula",
  },
];

const orders = ref([
  {
    id: 0,
    status: "В пути",
    orderName: "Картошка (1кг)",
    price: 89,
    currency: "RUB",
    comment: "Хорошая, крупная картошка",
    qty: 50,
    qtyShipped: 0,
    qtyPaid: 0,
    forPayment: function () {
      return (this.qtyShipped - this.qtyPaid) * this.price;
    },
  },
  {
    id: 0,
    status: "В пути",
    orderName: "Антибиотик",
    price: 1200,
    currency: "RUB",
    comment: "То, что мне нужно",
    qty: 1,
    qtyShipped: 1,
    qtyPaid: 0,
    forPayment: function () {
      return (this.qtyShipped - this.qtyPaid) * this.price;
    },
  },
  {
    id: 0,
    status: "Отменён",
    orderName: "Витами C",
    price: 344,
    currency: "RUB",
    comment: "Не доехало :(",
    qty: 50,
    qtyShipped: 0,
    qtyPaid: 0,
    forPayment: function () {
      return (this.qtyShipped - this.qtyPaid) * this.price;
    },
  },
]);
</script>

<template>
  <MainTable :columns="columns" :tableData="orders" />
</template>

<style scoped></style>

