<script> 
import { doc, getDocs, deleteDoc, addDoc, collection, onSnapshot, Timestamp } from 'firebase/firestore'
import vaquitas from '../utils/firebase.js'

export default {
  data() {
    return {
    }
  },
  props: {
    price: {
      type: Number,
      default: 0,
    },
    id: {
      type: String,
      default: '',
    },
    foundId: {
      type: String,
      default: '',
    },
  },
  methods:{
      removeItem() {
        deleteDoc(doc(collection(vaquitas, this.foundId, "prices"), this.id))
          .then(v => {
            console.log(v)
          }).catch(e => {
            this.alert = e
          })
    },
  }
}
</script>
<template>
      <li>
      {{ price }} 
      <button @click="removeItem()">X</button>
      </li>
</template>
