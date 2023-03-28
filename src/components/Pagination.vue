<template>
  <div class="pagination-container">
    <ul class="pagination">
      <li class="page-number" @click="PreviousOrNext('previous')">Prev</li>
      <li
        v-for="num in pageNumbers"
        :key="num"
        class="page-number"
        @click="paginate(num)"
      >
        {{ num }}
      </li>
      <li class="page-number" @click="PreviousOrNext('next')">Nex</li>
    </ul>
  </div>
</template>

<script>
import { EventBus } from "../main.js";
export default {
  name: "Pagination",
  data() {
    return {
      currentPage: 1,
      filterPokemon: 0,
    };
  },
  props: ["pokePerPage"],
  computed: {
    pageNumbers() {
      let pageNumbers = [];
      for (
        let i = 1;
        i <= Math.ceil(this.filterPokemon / this.pokePerPage);
        i++
      ) {
        pageNumbers.push(i);
      }
      return pageNumbers;
    },
    indexOfLastAndFirst() {
      let indexInfo = [];
      indexInfo[1] = this.currentPage * this.pokePerPage;
      indexInfo[0] = indexInfo[1] - this.pokePerPage;
      return indexInfo;
    },
  },
  watch: {
    currentPage: function () {
      EventBus.$emit("update-current-page", this.indexOfLastAndFirst);
    },
  },
  methods: {
    PreviousOrNext(action) {
      if (action === "previous") {
        if (this.currentPage !== 1) {
          this.currentPage--;
        }
      } else if (action === "next") {
        if (
          this.currentPage !== Math.ceil(this.filterPokemon / this.pokePerPage)
        ) {
          this.currentPage++;
        }
      } else {
        this.currentPage = 1;
        console.log("Invalid Page Action");
      }
    },
    paginate(page) {
      this.currentPage = page;
    },
  },
  created() {
    EventBus.$on("total-filter-pokemon", (value) => {
      this.filterPokemon = value;
    });
  },
};
</script>

<style scoped>
.pagination {
  list-style-type: none;
  display: flex;
  justify-content: center;
  align-items: center;
  margin-top: 20px;
  gap: 5px;
}

.pagination .page-number {
  font-size: 15px;
  font-weight: 600;
  color: #00a8ff;
  background: #fff;
  padding: 10px 10px;
  border-radius: 10px;
  cursor: pointer;
  transition: all 0.5s ease;
}

.pagination .page-number:hover {
  color: #fff;
  background: #00a8ff;
}

.pagination .active {
  color: #fff;
  background: #00a8ff;
}

.pagination .active:hover {
  color: #00a8ff;
  background: #fff;
}
</style>>