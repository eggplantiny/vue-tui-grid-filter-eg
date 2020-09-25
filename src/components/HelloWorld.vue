<template>
    <v-container fluid>
        <v-row>
            <v-col cols="12" class="text-center">
                <date-range-picker
                    v-model="dateRange"
                    :append-to-body="true"
                    :auto-apply="true"
                    @update="updateDate"
                >
                    <template v-slot:input="picker" style="min-width: 350px;">
                        {{ picker.startDate | date }} - {{ picker.endDate | date }}
                    </template>
                </date-range-picker>
            </v-col>
            <v-col cols="12">
                <grid
                    ref="grid"
                    :data="data"
                    :columns="columns"
                />
            </v-col>
        </v-row>
    </v-container>
</template>

<script>
import { Grid } from '@toast-ui/vue-grid'
import moment from 'moment'

import DateRangePicker from 'vue2-daterange-picker'
import 'vue2-daterange-picker/dist/vue2-daterange-picker.css'


export default {
    name: 'HelloWorld',
    components: {
        Grid,
        DateRangePicker
    },
    data () {
        return {
            dateRange: {
                startDate: moment().startOf('month'),
                endDate: moment()
            },
            columns: [
                {
                    name: 'createdAt',
                    header: '생성일',
                    filter: {
                        type: 'date',
                        operator: 'AND'
                    }
                },
                {
                    name: 'userName',
                    header: '사용자명'
                }
            ],
            data: [
                { createdAt: '2020-09-18', userName: 'eggplantiny1' },
                { createdAt: '2020-09-11', userName: 'eggplantiny2' },
                { createdAt: '2020-09-04', userName: 'eggplantiny3' },
                { createdAt: '2020-09-25', userName: 'eggplantiny4' },
            ]
        }
    },
    filters: {
        date (value) {
            return moment(value).format('YYYY.MM.DD')
        }
    },
    methods: {
        updateDate ({ startDate, endDate }) {
            this.$refs.grid.invoke('filter', 'createdAt', [
                { code: 'afterEq', value: startDate },
                { code: 'beforeEq', value: endDate }
            ])
        }
    }
}
</script>

<style>
.reportrange-text {
    border: none;
    border-radius: 4px;
    color: #1e1e1e;
    opacity: 0.8;
}

.col {
    -ms-flex-preferred-size: auto !important;
    flex-basis: auto !important;
}

.col {
    -ms-flex-preferred-size: auto !important;
    flex-basis: auto !important;
}

</style>
