<template>
  <div class="container">
    <input
      class="search-bar"
      type="text"
      placeholder="Search..."
      v-model="searchText"
    />
    <br />
    <div class="inventory-items">
      <div class="add-items-container">
        <router-link to="/inventory/add" class="add-item pure-button"
          >Add Item</router-link
        >
      </div>
      <table class="pure-table pure-table-horizontal">
        <thead>
          <tr>
            <th></th>
            <th>Name</th>
            <th
              v-for="(property, index) in inventoryProperties"
              v-bind:key="index"
            >
              {{ property }}
            </th>
            <th>Quantity</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="item in searchedItems" :key="item.id">
            <td>
              <router-link :to="$route.path + `/${item.id}`"
                ><i class="far fa-edit"></i
              ></router-link>
            </td>
            <td>{{ item["name"] }}</td>
            <td
              v-for="(property, index) in inventoryProperties"
              v-bind:key="index"
            >
              {{ item[property] }}
            </td>
            <td>{{ item["qty"] }}</td>
          </tr>
        </tbody>
      </table>
      <!-- <item-list :items="searchedItems" /> -->
      <div v-if="showModal" class="modal-route">
        <div class="modal-content">
          <router-view></router-view>
        </div>
      </div>
    </div>
    <div class="spacer"></div>
  </div>
</template>

<script>
//import ItemList from "../components/ItemList.vue";

export default {
  name: "InventoryHome",
  data() {
    return {
      showModal: false,
      searchText: "",
    };
  },
  computed: {
    searchedItems() {
      return this.searchText === ""
        ? this.$root.$data.store.inventories[
            this.$root.$data.store.activeInventory
          ]
        : this.$root.$data.store.inventories[
            this.$root.$data.store.activeInventory
          ].filter((x) => x.name.includes(this.searchText));
    },
    inventoryProperties() {
      return this.$root.$data.store.properties[
        this.$root.$data.store.activeInventory
      ];
    },
  },
  watch: {
    $route: {
      immediate: true,
      handler: function (newVal) {
        this.showModal = newVal.meta && newVal.meta.showModal;
      },
    },
  },
  components: {
    // ItemList,
  },
};
</script>

<style scoped>
.container {
  display: block;
  width: 100%;
  height: 100%;
}

.search-bar {
  font-size: 1.2em;
  line-height: 2em;
  border-radius: 1em;
  width: 50ch;
  padding-left: 10px;
  margin-top: 30px;
  margin-bottom: 35px;
}

.search-bar:focus {
  outline: transparent;
}

table {
  width: 100%;
}

th {
  text-align: center;
}

th,
td {
  border-bottom: 1px solid gray;
}

td > a {
  color: black;
}

td > a:hover {
  color: gray;
}

.inventory-items {
  width: 70%;
  margin-right: auto;
  margin-left: auto;
}

.add-items-container {
  text-align: end;
}

.add-item {
  font-weight: bold;
  border-radius: 5px;
  margin-bottom: 10px;
}

.add-item:hover {
  cursor: pointer;
}

.modal-route {
  width: 100%;
  height: 100%;
  position: fixed;
  top: 0;
  left: 0;
  background: rgba(0, 0, 0, 0.5);
}

.modal-content {
  width: 40%;
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  background: white;
}

.spacer {
  height: 200px;
  width: 100%;
  display: flex;
  justify-content: end;
}
</style>
