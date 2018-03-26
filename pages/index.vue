<template>
  <b-container>
    <b-row>
      <b-col>
        <h2>知乎无限 live </h2>
      </b-col>
    </b-row>
    <b-row>
      <b-col
        md="6"
        class="my-1">
        <b-form-group
          horizontal
          label="Filter"
          class="mb-0">
          <b-input-group>
            <b-form-input
              v-model="filter"
              placeholder="Type to Search" />
            <b-input-group-append>
              <b-btn
                :disabled="!filter"
                @click="filter = ''">Clear</b-btn>
            </b-input-group-append>
          </b-input-group>
        </b-form-group>
      </b-col>

      <b-col
        md="6"
        class="my-1">
        <b-pagination
          :total-rows="totalRows"
          :per-page="perPage"
          v-model="currentPage"
          class="my-0" />
      </b-col>

      <b-col
        md="6"
        class="my-1">
        <b-form-group
          horizontal
          label="Per page"
          class="mb-0">
          <b-form-select
            :options="pageOptions"
            v-model="perPage" />

        </b-form-group>
      </b-col>

      <b-col>
        <b-table
          striped
          hover
          :items="lives"
          :fields="fields"
          :sort-by.sync="sortBy"
          :sort-desc.sync="sortDesc"
          :current-page="currentPage"
          :per-page="perPage"
          :filter="filter"
          @filtered="onFiltered"
        >
          <template
            slot="id"
            slot-scope="data">
            {{ data.value }}
          </template>
          <template
            slot="subject"
            slot-scope="data">
            <a :href="`https://www.zhihu.com/lives/${data.item.id}`">
              {{ data.value }}
            </a>
          </template>
          <template
            slot="speaker"
            slot-scope="data">
            <a :href="`https://www.zhihu.com/people/${data.item.speaker_url}`">
              {{ data.value }}
            </a>
          </template>
        </b-table>
      </b-col>
    </b-row>
  </b-container>
</template>

<script>
import axios from 'axios';
import moment from 'moment';

export default {
  data() {
    const sortable = true;
    return {
      fields: [
        { key: 'id', sortable },
        { key: 'subject' },
        { key: 'starts_at', sortable, formatter: 'dateFormatter' },
        { key: 'fee', sortable, formatter: 'feeFormatter' },
        { key: 'participants', sortable },
        { key: 'score', sortable },
        { key: 'speaker', sortable },
      ],
      sortBy: 'id',
      sortDesc: true,
      currentPage: 1,
      perPage: 10,
      pageOptions: [10, 20, 100],
      filter: null,
    };
  },
  async asyncData({ error }) {
    const url = 'https://raw.githubusercontent.com/calpa/zhihu-unlimited-live/master/db.json';
    try {
      const { data } = await axios.get(url);
      const lives = Object.values(data.live);

      return {
        lives,
        totalRows: lives.length,
      };
    } catch (err) {
      error({ statusCode: 404, mesesage: 'Items not found' });
    }
    return { };
  },
  methods: {
    dateFormatter(value) {
      return moment.unix(value).format('YYYY-MM-DD');
    },
    feeFormatter(value) {
      return `${value / 100}`;
    },
    onFiltered(items) {
      this.totalRows = items.length;
      this.currentPage = 1;
    },
  },
};
</script>

<style>
.container {
    padding-top: 20px;
}
</style>
