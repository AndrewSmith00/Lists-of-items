<template>
  <button type="button" v-if="isSorted" v-on:click="shuffle">Перемешать</button>
  <button type="button" v-if="!isSorted" v-on:click="sort">
    Отсортировать
  </button>
  <div class="items" v-for="(item, i) in list" v-bind:key="i">
    <div class="item-container" v-for="(items, j) of item" v-bind:key="j">
      <div v-on:click="$emit('deleteItem', itemNum = items.parentNum)"
        class="item-container__item"
        :style="'background-color:' + items.color + ';'"
      ></div>
    </div>
  </div>
</template>

<script>
export default {
  props: ["items"],
  data() {
    return {
      list: [],
      isSorted: true,
    };
  },

  methods: {
    createList() {
      let list = [];
      let i = 0;
      for (let item of this.items) {
        list[i] = [];
        for (let j = 0; j < item.amount; j++) {
          list[i].push({ color: item.color, parentNum: i });
        }
        i++;
      }
      return list;
    },

    shuffle() {
      let newList = [];
      for (let item of this.list) {
        Array.prototype.push.apply(newList, item);
        console.log(JSON.stringify(newList));
      }

      for (let i = newList.length - 1; i > 0; i--) {
        let j = Math.floor(Math.random() * (i + 1));
        [newList[i], newList[j]] = [newList[j], newList[i]]
      }

      this.list = [newList]
      this.isSorted = false;
    },

    sort() {
        this.list = this.createList();
        this.isSorted = true;
    }
  },

  watch: {
    items() {
      if (this.isSorted == false) {
        this.list = this.createList();
        this.shuffle();
      } else
      this.list = this.createList();
    },
  },

  mounted() {
    this.list = this.createList();
  },
};
</script>

<style>
.item-container {
  display: inline-block;
}

.item-container__item {
  width: 8px;
  height: 8px;
  margin: 0 1px;
  cursor: pointer;
}
</style>
