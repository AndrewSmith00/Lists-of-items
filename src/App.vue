<template>
  <body>
    <h2
      v-on:click="isOpen = !isOpen"
      style="cursor: pointer; width: fit-content"
    >
      <img src="static/imgs/arrow-down.png" v-if="isOpen" />
      <img src="static/imgs/arrow-right.png" v-if="!isOpen" />
      Lists
    </h2>
    <div class="lists" v-if="isOpen">
      <div v-for="(list, i) in Lists" v-bind:key="i">
        <span class="list__title-block">
          <img
            src="static/imgs/arrow-down.png"
            v-if="list.isOpen"
            v-on:click="list.isOpen = !list.isOpen"
          />
          <img
            src="static/imgs/arrow-right.png"
            v-if="!list.isOpen"
            v-on:click="list.isOpen = !list.isOpen"
          />
          <input
            type="checkbox"
            :indeterminate="list.checked == 'indeterminate'"
            :checked="list.checked"
            v-on:change="checkList(i)"
          />
          <span v-on:click="list.isOpen = !list.isOpen">List {{ i + 1 }}</span>
        </span>
        <List
          :items="list.items"
          :isChecked="list.checked"
          @itemChecked="(itemColor) => checkItem(i, itemColor)"
          v-if="list.isOpen"
        >
        </List>
      </div>
    </div>

    <div class="filtered-lists">
      <div
        class="filtered-lists__items"
        v-for="(items, i) in filteredLists"
        v-bind:key="i"
      >
        List {{ i + 1 }}
        <ItemsInList :items="items" 
         @deleteItem="(itemNum) => deleteItem(i, itemNum)">
        </ItemsInList>
      </div>
    </div>
  </body>
</template>

<script>
import List from "./components/ListOfItems.vue";
import ItemsInList from "./components/ItemsInList.vue";
export default {
  name: "App",
  components: { List, ItemsInList },

  data() {
    return {
      Lists: [],
      filteredLists: [],
      isOpen: true,
    };
  },

  methods: {
    createLists(amountOfLists) {
      let lists = [];
      let colors = [
        "yellow",
        "red",
        "green",
        "blue",
        "black",
        "brown",
        "orange",
        "pink",
        "gray",
      ];

      for (let i = 0; i < amountOfLists; i++) {
        lists[i] = { items: [], isOpen: false, checked: false };

        let sizeOfList = Math.floor(Math.random() * 10);
        if (sizeOfList < 4) sizeOfList += 4;

        for (let j = 0; j < sizeOfList; j++) {
          let itemsColor = Math.floor(Math.random() * colors.length);
          let itemsAmount = Math.ceil(Math.random() * 40);

          lists[i].items.push({
            color: colors[itemsColor],
            amount: itemsAmount,
            checked: false,
          });
        }
      }

      return lists;
    },

    checkList(listNum) {
      this.Lists[listNum].checked = !this.Lists[listNum].checked;
      for (let item of this.Lists[listNum].items) {
        item.checked = this.Lists[listNum].checked;
      }
    },

    checkItem(listNum, itemNum) {
      this.Lists[listNum].items[itemNum].checked =
        !this.Lists[listNum].items[itemNum].checked;
    },

    filterLists() {
      let filtered = [];
      for (let list of this.Lists) {
        filtered.push(list.items.filter((item) => item.checked));
      }
      return filtered;
    },

    deleteItem(listNum, itemNum) {
      console.log(listNum + '|' + itemNum)
      this.filteredLists[listNum][itemNum].amount--
    }
  },

  watch: {
    Lists: {
      handler: function () {
        this.filteredLists = this.filterLists();
        for (let i = 0; i < this.Lists.length; i++) {

          if (this.Lists[i].items.length == this.filteredLists[i].length) {
            this.Lists[i].checked = true;
          } else if (this.filteredLists[i].length == 0){
            this.Lists[i].checked = false;
          } else {
            this.Lists[i].checked = "indeterminate";
          }
        }

        localStorage.setItem("lists", JSON.stringify(this.Lists));
      },
      deep: true,
    },
  },

  mounted() {
    if (localStorage.getItem("lists") === null) {
      this.Lists = this.createLists(5);
      localStorage.setItem("lists", JSON.stringify(this.Lists));
    } else {
      this.Lists = JSON.parse(localStorage.getItem("lists"));
    }

    this.filteredLists = this.filterLists();
  },
};
</script>

<style>
body {
  width: 100%;
  display: inline-block;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
}

.lists {
  width: 55%;
  float: left;
}

.list__title-block {
  width: fit-content;
  cursor: pointer;
}

.filtered-lists {
  width: 40%;
  float: right;
}

.filtered-lists__items {
  margin: 10px 50px 10px 0;
  padding: 10px;
  border: 1px solid #bbb;
}
</style>
