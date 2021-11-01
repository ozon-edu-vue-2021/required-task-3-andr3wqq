<template>
    <div class="map">
        <h3>Карта офиса</h3>

        <div
            v-if="!isLoading"
            class="map-root">
            <MapSVG ref="map"/>
            <TableSVG v-show="false" ref="table"/>
        </div>
        <div v-else>Loading...</div>
    </div>
</template>

<script>
import MapSVG from '@/assets/images/map.svg';
import TableSVG from '@/assets/images/workPlace.svg';
import * as d3 from 'd3';
import tables from '@/assets/data/tables.json';
import legend from '@/assets/data/legend.json';

export default {
    props: {
        tableId: {
            type: Number,
            default: null,
        },
    },
    components: {
        MapSVG,
        TableSVG,
    },
    data() {
        return {
            isLoading: false,
            mapSVG: null,
            tableSVG: null,
            g: null,
            tables: [], // все столы
        };
    },
    created() {
        this.tables = tables;
    },
    mounted() {
        this.isLoading = true;

        this.mapSVG = d3.select(this.$refs.map);
        this.g = this.mapSVG.select('g');
        this.tableSVG = d3.select(this.$refs.table); // получаем шаблон стола
        if (this.g) {
            this.drawTables();
        } else {
            alert('SVG is incorrect');
        }

        this.isLoading = false;
    },
    methods: {
        drawTables() {
            // создаем группу для рабочик мест
            const svgTablesGroupPlace = this.g
                .append('g')
                .classed('groupPlaces', true);

            this.tables.map((table) => {
                // создает группу для рабочего стола
                const targetSeat = svgTablesGroupPlace
                    .append('g')
                    .attr('transform', `translate(${table.x}, ${table.y}) scale(0.5)`)
                    .attr('id', table._id)
                    .classed('employer-place', true);

                // устанавливает стол в группу
                targetSeat
                    .append('g')
                    .attr('transform', `rotate(${table.rotate || 0})`)
                    .attr('group_id', table.group_id)
                    .classed('table', true)
                    .html(this.tableSVG.html())
                    .attr(
                        // устанавливаем цвет подразделения
                        'fill',
                        legend.find((it) => it.group_id === table.group_id)?.color ??
                        'transparent'
                    )
                    .on('click', () => {
                        this.$emit('update:table-id', table._id);
                    });
            });
        },
    },
};
</script>

<style scoped>
.map {
    height: 100%;
    width: 100%;
    padding: 24px;
    overflow: hidden;
    box-sizing: border-box;
    display: flex;
    flex-direction: column;
}

.map-root {
    height: 100%;
    width: 100%;
    overflow: hidden;
    box-sizing: border-box;
}

h3 {
    margin-top: 0px;
}

::v-deep svg {
    height: 100%;
    width: 100%;
}

::v-deep .table {
    cursor: pointer;
}
</style>
