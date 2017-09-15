<script>
import { mapState } from 'vuex'
import Highcharts from 'highcharts'
import _ from 'lodash'

export default {
    computed: mapState({
        list: state => state.list
    }),
    watch: {
        list(){
            this.dataSource()
        }
    },
    methods: {
        dataSource(){
            const countries = this.list.map(item => item.country)
            const base = _(countries)
                .countBy()
                .map((value, key) => ({ key, value}))
                .orderBy(['value'], ['desc'])
                .value()

            const categories = base.map(item => item.key)
            const values = base.map(item => item.value)
            this.setup({categories, values})
        },
        setup(obj){
            const { categories, values } = obj

            Highcharts.chart('container-for-countries', {
                chart: {
                    type: 'column'
                },
                title: {
                    text: 'Countries'
                },
                subtitle: {
                    text: 'Source: teste'
                },
                xAxis: {
                    categories: categories,
                    crosshair: true
                },
                yAxis: {
                    min: 0,
                    title: {
                        text: 'Quantidade'
                    }
                },
                series: [{
                    name: 'Quantidade',
                    data: values

                }]
            });
        }
    }
}
</script>

<template>
    <div id="container-for-countries"></div>
</template>
