<template>
    <div
        v-if="loading"
        :class="(loading ? 'defi-loading defi-loading--full' : '')"/>
    <section v-else>

        <div
            v-if="total == 0"
            class="defi-empty">
            <i class="fa fa-cube defi-empty__icon"/>
            <p class="defi-empty__description">No reward found</p>
        </div>

        <p
            v-if="total > 0"
            class="defi-total-items">{{ _nFormatNumber('reward', 'rewards', total) }}</p>

        <table-base
            v-if="total > 0"
            :fields="fields"
            :items="items"
            class="defi-table--reward defi-table--reward-by-epoch">
            <template
                slot="address"
                slot-scope="props">
                <nuxt-link
                    :to="{name: 'address-slug', params: {slug: props.item.address}}"
                    class="text-truncate">{{ props.item.address }}</nuxt-link>
            </template>
            <template
                slot="validator"
                slot-scope="props">
                <nuxt-link
                    :to="{name: 'address-slug', params: {slug: props.item.validator}}"
                    class="text-truncate">{{ props.item.validator }}</nuxt-link>
            </template>
            <template
                slot="validatorName"
                slot-scope="props">
                <nuxt-link
                    :to="{name: 'address-slug', params: {slug: props.item.validator}}"
                    class="text-truncate">{{ props.item.validatorName }}</nuxt-link>
            </template>
            <template
                slot="signNumber"
                slot-scope="props">
                {{ props.item.signNumber }}
            </template>
            <template
                slot="reward"
                slot-scope="props">
                {{ formatNumber(props.item.reward) }}
            </template>

            <template
                slot="timestamp"
                slot-scope="props">
                <span
                    v-b-tooltip.hover
                    :title="$moment(props.item.rewardTime).format('lll')">
                    {{ $moment(props.item.rewardTime).fromNow() }}</span>
            </template>

        </table-base>

        <b-pagination-nav
            v-if="total > 0 && total > perPage"
            v-model="currentPage"
            :total-rows="total"
            :per-page="perPage"
            :number-of-pages="pages"
            :link-gen="linkGen"
            :limit="7"
            align="center"
            class="defi-pagination"
            @change="onChangePaginate"
        />
    </section>
</template>
<script>
import mixin from '~/plugins/mixin'
import TableBase from '~/components/TableBase'

export default {
    components: {
        TableBase
    },
    mixins: [mixin],
    props: {
        epoch: {
            type: Number,
            default: 0
        },
        reason: {
            type: String,
            default: ''
        },
        page: {
            type: Object,
            default: () => {
                return {}
            }
        },
        parent: {
            type: String,
            default: ''
        }
    },
    data: () => ({
        fields: {
            address: { label: 'Address' },
            validator: { label: 'MasterNode' },
            validatorName: { label: 'MasterNode Name' },
            signNumber: { label: 'Sign number' },
            // lockBalance: { label: 'Lock balance' },
            reward: { label: 'Reward' },
            timestamp: { label: 'Age' }
        },
        loading: true,
        pagination: {},
        total: 0,
        items: [],
        currentPage: 1,
        perPage: 20,
        pages: 1
    }),
    watch: {
        $route (to, from) {
            const page = this.$route.query.page
            this.onChangePaginate(page)
        }
    },
    async mounted () {
        this.getDataFromApi()
    },
    methods: {
        async getDataFromApi () {
            let self = this

            self.currentPage = parseInt(this.$route.query.page)

            // Show loading.
            self.loading = true

            let params = {
                page: self.currentPage || 1,
                limit: self.perPage,
                reason: self.reason
            }

            let query = this.serializeQuery(params)
            let { data } = await this.$axios.get('/api/rewards/epoch/' + self.epoch + '?' + query)
            self.items = data.items
            self.total = data.total
            self.currentPage = data.currentPage
            self.pages = data.pages
            self.page.rewardTime = data.total
            self.perPage = data.perPage

            // Hide loading.
            self.loading = false

            return data
        },
        onChangePaginate (page) {
            let self = this
            self.currentPage = page

            self.getDataFromApi()
        },
        linkGen (pageNum) {
            return {
                query: {
                    page: pageNum
                },
                hash: this.parent
            }
        }
    }
}
</script>
