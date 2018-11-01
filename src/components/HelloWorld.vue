<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <input id="doInput" v-model="newItem" placeholder="do what?" @keydown.enter="addItem">
    <ul>
      <li v-for="(item, index) in items" :key="item.id">
        <input type="checkbox" :checked="item.isFinished" @click="toggle(item)"><span :class="{deleteLine: item.isFinished}">{{ index + 1 }}.{{ item.content }}</span><span class="finish" v-show="item.isFinished">finished</span><span class="delete" @click="delItem(item, index)">delete</span>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  name: "HelloWorld",
  props: {
    msg: String
  },
  data() {
    return {
      newItem: "",
      items: JSON.parse(window.localStorage.getItem("itemList")) || [],
      nextItemId: 1
    };
  },
  methods: {
    addItem() {
      this.items.push({
        id: this.nextItemId++,
        content: this.newItem,
        isFinished: false
      });
      this.newItem = "";
    },
    toggle(item) {
      item.isFinished = !item.isFinished;
    },
    delItem(item, index) {
      this.items.splice(index, 1);
    }
  },
  watch: {
    items: {
      handler: function() {
        window.localStorage.setItem("itemList", JSON.stringify(this.items));
      },
      deep: true
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.hello {
  margin: 0 auto;
  width: 600px;
}
#doInput {
  height: 25px;
  width: 300px;
}
ul {
  text-align: left;
  margin-left: 100px;
  list-style-type: none;
}
.finish {
  margin-left: 10px;
  background-color: red;
}
.deleteLine {
  text-decoration: line-through;
}
.delete {
  margin-left: 10px;
  cursor: pointer;
  background-color: orange;
  visibility: hidden;
}
li:hover .delete {
  visibility: visible;
}
</style>
