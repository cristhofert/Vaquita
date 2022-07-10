<script>
import vaquitas from "../utils/firebase.js";
import { getDocs } from 'firebase/firestore'
import Vaquita from "./Vaquita.vue";

export default {
  data() {
    return {
      list: [],
      alert: "",
      foundId: '',
      foundName: "...",
    }
  },
  async mounted() {
    getDocs(vaquitas)
    .then(c => {
      this.list = c.docs.map(doc => ({ ...doc.data(), id: doc.id}) )
      this.setFoundId(this.list.id, this.list.name)
    } 
    )
    .catch(e => {
      this.alert = e
    })
  },
  methods: {
    setFoundId(id, name) {
      this.foundId = id;
      this.foundName = name;
    }
  },
  components: {
    Vaquita,
  }
}
</script>
<template>
    {{ alert == "" ? "" : alert }}
    <ul>
      <li v-for="item in list" :key="item.id">
        <button @click="e => setFoundId(item.id, item.name)">
          {{ item.name }}
        </button>
      </li>
    </ul>
    <Vaquita v-show="foundId!=''" :found-id="foundId" :name="foundName" />
</template>