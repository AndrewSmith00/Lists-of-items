<template>
  <div v-for="(item, i) in items" v-bind:key="i">
    <div class="item">
      <div class="item__title">
        <input
          type="checkbox"
          :checked="item.checked"
          v-bind:value="items"
          v-on:change="$emit('itemChecked', (itemNum = i))"
        />
        Item {{ i + 1 }}
      </div>
        <input
         class="prop-input"
          type="number"
          placeholder="Количество"
          onfocus="this.placeholder=''"
          v-model="item.amount"
        />

        <input
        class="prop-input"
        type="color"
        placeholder="Цвет"
        onfocus="this.placeholder=''"
        v-model="item.color"
      />

      <div class="item__properties">
        <div class="amount">{{ item.amount }}</div>
        <div
          class="color"
          :style="'background-color:' + item.color + ';'"
        ></div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: ["items"],
  data() {

  },
};
</script>

<style>
.item {
  display: flex;
  width: 100%;
}

.item.title,
.prop-input,
.item__properties {
  margin-left: 3%;
}

.item__title {
  padding-left: 5%;
}

.item__properties {
  display: flex;
  align-items: center;
}

.prop-input {
  width: 15%;
  height: 20%;
  font-size: 10px;
}

.prop-input>input {
  width: 100%;

}

.color {
  width: 10px;
  height: 10px;
  margin-left: 10px;
}
</style>
