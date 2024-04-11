<template>
    <div class="card text-center m-3">
        <h3 class="card-header">Vue.js + Node - Server Side Pagination Example</h3>
        <div class="card-body">
            <div v-for="item in pageOfItems" :key="item.id">{{item.name}}</div>
        </div>
        <div class="card-footer pb-0 pt-3">
            <ul v-if="pager.pages && pager.pages.length" class="pagination">
                <li :class="{'disabled':pager.currentPage === 1}" class="page-item previous-item">
                    <router-link :to="{ query: { page: pager.currentPage - 1 }}" class="page-link">Previous</router-link>
                </li>
                <li class="page-item number-item">
                  {{ pageOfItems[0].id  }}  to  {{  pageOfItems[pageOfItems.length-1].id }}
                </li>
                <li :class="{'disabled':pager.currentPage === pager.totalPages}" class="page-item next-item">
                    <router-link :to="{ query: { page: pager.currentPage + 1 }}" class="page-link">Next</router-link>
                </li>
            </ul>
        </div>
    </div>
</template>

<script>
export default {
    data () {
        return {
            pager: {},
            pageOfItems: []
        }
    },
    watch: {
        '$route.query.page': {
            immediate: true,
            handler(page) {
                page = parseInt(page) || 1;
                if (page !== this.pager.currentPage) {
                    fetch(`/api/items?page=${page}`, { method: 'GET' })
                        .then(response => response.json())
                        .then(({pager, pageOfItems}) => {
                            this.pager = pager;
                            this.pageOfItems = pageOfItems;
                        });
                }
            }
        }
    }
}
</script>
