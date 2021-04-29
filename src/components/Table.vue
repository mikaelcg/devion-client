<template>
  <v-data-table
    v-bind="$attrs"
    :loading="loading"
    :headers="computedHeaders"
    :items="items"
    :items-per-page="itemsPerPage"
    class="Table elevation-1"
  >
    <template #item.actions="{ item }">
      <v-icon class="mr-2" @click="editItem(item.id)" v-if="!hideEdit">
        mdi-pencil
      </v-icon>
      <v-icon @click="deleteItem(item.id)" v-if="!hideDelete">
        mdi-delete
      </v-icon>
    </template>
    <template #no-data> </template>
  </v-data-table>
</template>

<script>
export default {
  name: "Table",
  props: {
    value: {
      type: Array,
      required: false,
      default: null
    },
    headers: {
      type: Array,
      required: true
    },
    model: {
      type: Object,
      required: false
    },
    items: {
      type: Array,
      required: true
    },
    itemsPerPage: {
      type: Number,
      required: false,
      default: 10
    },
    hideEdit: {
      type: Boolean,
      required: false
    },
    hideDelete: {
      type: Boolean,
      required: false
    }
  },
  data() {
    return {
      loading: false,
      loaded: false
    };
  },
  created() {
    this.handleModel();
  },
  methods: {
    async handleModel() {
      if (this.model) {
        const response = await this.loadCollection();
        this.$emit("input", response);
      }
    },

    editItem(id) {
      this.$router.push({
        name: `${this.$route.name}.edit`,
        params: { id: id }
      });
    },
    async deleteItem(id) {
      await this.model.deleteRecord(id);
      await this.model.loadCollection();
    },
    async loadCollection(page = 0, filter = null, sort = null) {
      if (this.loading) {
        return null;
      }

      this.loaded = false;

      // Exibe o spinner apenas após 50ms
      var t = setTimeout(() => {
        this.loading = true;
      }, 50);

      try {
        // if (!filter && findKey(this.getFilters(), "value")) {
        //   filter = this.getFilters();
        // }

        // if (page === 0) {
        //   this.resetPage();
        // }

        const response = await this.model.loadCollection(page, filter, sort);
        this.$emit("input", response);
        return Promise.resolve(response);
      } finally {
        clearTimeout(t);
        this.loaded = true;
        this.loading = false;
      }
    }
  },
  computed: {
    computedHeaders() {
      return [
        ...this.headers,
        { text: "Ações", value: "actions", sortable: false, align: "end" }
      ];
    }
  }
};
</script>