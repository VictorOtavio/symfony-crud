<template>
  <div>
    <main class="main" @click="actionClick">
      <Face :mood="faceMood" />
      <Modal :show.sync="showModal" />
    </main>
    <Copyright />
  </div>
</template>

<script>
import Face from "@/components/Face";
import Modal from "@/components/Modal";
import Copyright from "@/components/Copyright";
import { resolveMetaInfo } from "@/services/MetaInfo";

export default {
  name: "WindowFace",

  components: {
    Face,
    Modal,
    Copyright
  },

  data() {
    return {
      showModal: false
    };
  },

  methods: {
    /**
     * Generic click that triggers page actions.
     * @param {Object} $event
     */
    actionClick($event) {
      // Ignores the click on any element other than the .main
      if (!$event.target.classList.contains("main")) {
        return;
      }

      // If the mood is different from sad, better be sad soon...
      if (this.faceMood !== "sad") {
        this.$store.dispatch("sadFace");
      }
      // If the mood is sad, tell a joke...
      else if (this.faceMood === "sad") {
        this.showModal = true;
      }
    }
  },

  computed: {
    faceMood() {
      return this.$store.state.faceMood;
    }
  },

  metaInfo() {
    return resolveMetaInfo(this.faceMood);
  }
};
</script>

<style scoped>
.main {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 100vw;
  height: calc(100vh - 60px);
}

@media (max-width: 768px) {
  .main {
    display: block;
  }
}
</style>
