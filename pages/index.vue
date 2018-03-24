<template>
  <section class="container">
    <div>
        <b-table 
            striped 
            hover 
            :items="lives" 
            :fields="fields"
            :sort-by.sync="sortBy"
            :sort-desc.sync="sortDesc"
            >
            <template slot="subject" slot-scope="data">
                <a :href="`https://www.zhihu.com/lives/${data.item.id}`">
                    {{data.value}}
                </a>
            </template>
        </b-table>
    </div>
  </section>
</template>

<script>
import axios from 'axios';
import BootstrapVue from 'bootstrap-vue';
import moment from 'moment';


export default {
    async asyncData({ error }) {
        const url = 'https://raw.githubusercontent.com/calpa/zhihu-unlimited-live/master/db.json'
        try {
            const { data } = await axios.get(url);
            const lives = Object.values(data.live);
            const sortable = true;
            const fields = [
                { key:'id', sortable},
                { key: 'subject'},
                { key: 'starts_at', sortable, formatter: "dateFormatter" },
                { key: 'fee', sortable, formatter: "feeFormatter" },
                { key: 'participants', sortable},
                { key: 'score', sortable }
            ];
            const sortBy = 'id';
            const sortDesc = true;

            return { lives, fields, sortBy, sortDesc };
        } catch (err) {
            error({ statusCode: 404, mesesage: 'Items not found'});
        }
    },
    methods: {
        dateFormatter(value) {
            return moment.unix(value).format('YYYY-MM-DD');
        },
        feeFormatter(value) {
            return `${value/100}`;
        }
    }
}
</script>

<style>
.container {
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
}

.title {
  font-family: "Quicksand", "Source Sans Pro", -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, "Helvetica Neue", Arial, sans-serif; /* 1 */
  display: block;
  font-weight: 300;
  font-size: 100px;
  color: #35495e;
  letter-spacing: 1px;
}

.subtitle {
  font-weight: 300;
  font-size: 42px;
  color: #526488;
  word-spacing: 5px;
  padding-bottom: 15px;
}

.links {
  padding-top: 15px;
}
</style>
