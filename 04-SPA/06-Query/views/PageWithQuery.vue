<template>
  <div class="container">
    <meetups-view
      :date.sync="query.date"
      :participation.sync="query.participation"
      :view.sync="query.view"
      :search.sync="query.search"
    />
  </div>
</template>

<script>
import MeetupsView from '../components/MeetupsView';

const default_ = {
  view: 'list',
  date: `all`,
  participation: `all`,
  search: ``,
};

export default {
  name: 'PageWithQuery',
  components: { MeetupsView },

  data() {
    return {
      query: { ...default_ },
    };
  },

  watch: {
    $route: {
      immediate: true,
      deep: true,
      handler(value) {
        this.query = Object.assign({}, default_, value.query);
      },
    },
    query: {
      deep: true,
      handler(value) {
        let route = Object.assign({}, { query: Object.assign({}, value) });

        for (let key in route.query) {
          if (route.query[key] === default_[key]) {
            delete route.query[key];
          }
        }
        this.$router.push(route).catch((err) => {
          if (err.name !== 'NavigationDuplicated') {
            throw err;
          }
        });
      },
    },
  },
};
</script>

<style scoped></style>
