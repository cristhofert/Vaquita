<script>
import Price from './price.vue'
import { doc, getDocs, deleteDoc, addDoc, collection, onSnapshot, Timestamp } from 'firebase/firestore'
import vaquitas from '../utils/firebase.js'

export default {
  props: {
    foundId: String,
    name: String,
  },
  data() {
    return {
      items: [],
      price: 0,
      total: 0,
      alert: "",
    }
  },
  methods: {
    getTotal() {
      this.total = this.items.reduce((prev, item) => {
        return prev + item
      }, 0)
    },
    addItem() {
      if (this.price > 0){ 
        addDoc(collection(vaquitas, this.foundId, "prices"), {
          price: this.price,
          created_at: Timestamp.now(),
        }).then(v => {
          this.price = 0
        }).catch(e => {
          this.alert = e
        })
        }
    },
    loadData() {
      const unsubscribe = onSnapshot(
      collection(vaquitas, this.foundId, "prices"),
      snapshot => {
        this.items = snapshot.docs.map(doc => ({ ...doc.data(), id: doc.id }) )
      }
    )
    }
  },
  components: { Price },
  mounted() {
    console.log("🐮 Mounted")
    if (this.foundId!='') {
      this.loadData()
    }
},
  watch: {
    foundId(newId) {
      console.log("🐮 foundId changed")
      this.loadData()
    },
  },
}
</script>
<template>
  <div v-if="alert != '' ">
      <h6>{{ alert }}</h6>
    </div>
  <h5 v-if="name">{{ name }}</h5>
  <input
    type="number"
    v-model="price"
    placeholder="Mount"
    @keyup.enter="addItem()"
  />
  <button @click="addItem()">+</button>
  <ul>
    <Price
      v-for="item in items"
      :key="item.id"
      :price="item.price"
      :id="item.id"
      :foundId="foundId"
    />
    <li>Total: {{ total }}</li>
    <li>Mitad: {{ total / 2 }}</li>
  </ul>
</template>
<style></style>
