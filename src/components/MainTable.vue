<script setup>
// Личные заметки от себя:
// NOTE:
// - Формула работает не лучшем образом, нет проверки на тип, статус, значения используемых полей.
// - В рамках задания не стал выносить типы полей в отдельные компоненты. Тут их всего 3 и они не "мешаются"
// - Так же я бы отображал данные, которые мы хотим видеть в таблице, через слоты (хотя бы для названия столбцов), это сделало бы таблицу более универсальной
// - Обошёлся минимальными стилями, надеюсь стилизация не была частью задания.

// NOTE: Тут, по хорошему, должен быть еще какой-нибудь statusId.
// be like: { title: "В пути", statusId: 0}
const statusOptions = ["В пути", "Олпачено", "Отменён"];

const props = defineProps({
  columns: {
    type: Array,
    validator(value) {
      let valid = true;

      // NOTE: Сомневаюсь в необходимости проверки ??
      // Так хотя бы выведет какое-то предупреждение, если, допустим, в массив попадёт строка
      value.forEach((v) => {
        if (!v || typeof v !== "object" || !v.hasOwnProperty("isReadOnly") || !v.hasOwnProperty("fieldType")) {
          valid = false;
        }
      });
      return valid;
    },
  },
  tableData: {
    type: Array,
    default: () => [],
  },
});
</script>

<template>
  <table>
    <thead>
      <tr>
        <th v-for="column in columns" :key="column.objKey">
          {{ column.title }}
        </th>
      </tr>
    </thead>
    <tbody>
      <tr v-for="row in tableData" :key="row.id">
        <td v-for="column in columns">
          <!-- Типы полей -->
          <div v-if="column.fieldType === 'select'">
            <select v-model="row[column.objKey]" name="select" :id="row.id" :disabled="column.isReadOnly">
              <option v-for="option in statusOptions" :value="option">
                {{ option }}
              </option>
            </select>
          </div>

          <div v-if="column.fieldType === 'input'">
            <input v-model="row[column.objKey]" type="text" :disabled="column.isReadOnly" />
          </div>

          <div v-if="column.fieldType === 'formula'">
            <!-- NOTE: 
             Не знаю, нужно ли было реализовывать возможность редактирования этого поля?
             Должно ли было это выглядеть как в Excel? A1+A2 -->
            <input :value="row.forPayment()" type="text" :disabled="column.isReadOnly" />
          </div>

          <!-- TODO: Вариант когда нет типа поля? -->
          <!-- <div>{{ row[column.objKey] }}</div> -->
        </td>
      </tr>
      <tr v-if="!tableData.length">
        Нет заказов
      </tr>
    </tbody>
  </table>
</template>

<style scoped>
table {
  border: 1px solid white;
  border-collapse: collapse;
}
th,
td {
  padding: 20px;
  border: 1px solid white;
}
</style>

