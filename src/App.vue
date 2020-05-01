<template>
  <v-app>
    <v-content class="pa-3">
      <v-data-table
        :headers="headers"
        disable-pagination
        disable-sort
        disable-filtering
        hide-default-footer
        hide-default-header
        class="elevation-1"
      >
        <template slot="header" slot-scope="prop">
          <thead>
            <tr class="header-row">
              <th v-for="(header, headerIndex) in prop.props.headers" :key="headerIndex" :class="header.customClass">
                {{ header.text }}
                <template v-if="header.text">
                  <br>
                  (<span :class="header.color">{{ header.day }}</span>)
                </template>
              </th>
            </tr>
          </thead>
        </template>
        <template slot="body">
          <tbody>
            <!--
            <tr>
              <td rowspan="2">
                예약률
              </td>
              <td>
                전체
              </td>
              <template v-for="(header, headerIndex) in headers">
                <td v-if="headerIndex > 1" :key="headerIndex">
                  {{ dataSet[header.saleYmd].data1 }}
                </td>
              </template>
            </tr>
            <tr>
              <td>
                기본
              </td>
              <template v-for="(header, headerIndex) in headers">
                <td v-if="headerIndex > 1" :key="headerIndex">
                  {{ dataSet[header.saleYmd].data2 }}
                </td>
              </template>
            </tr>
            <tr>
              <td rowspan="3">
                <v-chip color="primary">{{ memInd }}</v-chip>
                <br>
                {{ memIndName }}
              </td>
              <td>
                기준금액
              </td>
              <template v-for="(header, headerIndex) in headers">
                <td v-if="headerIndex > 1" :key="headerIndex">
                  {{ dataSet[header.saleYmd].data3 }}
                </td>
              </template>
            </tr>
            <tr>
              <td>
                현재금액
              </td>
              <template v-for="(header, headerIndex) in headers">
                <td v-if="headerIndex > 1" :key="headerIndex">
                  {{ dataSet[header.saleYmd].data4 }}
                </td>
              </template>
            </tr>
            <tr>
              <td>
                레벨적용
              </td>
              <template v-for="(header, headerIndex) in headers">
                <td v-if="headerIndex > 1" :key="headerIndex">
                  {{ dataSet[header.saleYmd].data5 }}
                </td>
              </template>
            </tr>
            <tr>
              <td colspan="2">
                재고량 전체
              </td>
              <template v-for="(header, headerIndex) in headers">
                <td v-if="headerIndex > 1" :key="headerIndex">
                  {{ dataSet[header.saleYmd].data6 }}
                </td>
              </template>
            </tr>
            -->
            <tr v-for="(rowLabel, rowLabelIndex) in rowLabels" :key="rowLabelIndex">
              <td
                class="header-col sticky"
                :colspan="rowLabel.colspan"
                :rowspan="rowLabel.rowspan"
                v-if="rowLabel.rowspan > 1 || rowLabel.colspan > 1">
                <template v-if="rowLabel.isChip">
                  <v-chip color="primary">
                    {{ rowLabel.chipText }}
                  </v-chip>
                  <span v-html="rowLabel.text"></span>
                </template>
                <template v-else>
                  {{ rowLabel.text }}
                </template>
              </td>
              <template v-if="rowLabel.colspan < 2">
                <td v-if="rowLabel.child && rowLabel.child.text" class="header-col second sticky">
                  {{ rowLabel.child.text }}
                </td>
                <td v-else-if="!rowLabel.child" class="header-col second sticky">
                  {{ rowLabel.text }}
                </td>
              </template>
              <template v-for="(header, headerIndex) in headers">
                <td v-if="headerIndex > 1 && rowLabel.child" :key="headerIndex" class="custom-col">
                  {{ dataSet[header.saleYmd][rowLabel.child.dataKey] }}
                </td>
                <td v-else-if="headerIndex > 1 && !rowLabel.child" :key="headerIndex" class="custom-col">
                  {{ dataSet[header.saleYmd][rowLabel.dataKey] }}
                </td>
              </template>
              <!--
              <td v-for="(saleYmd, saleYmdIndex) in headers" :key="saleYmdIndex">
                {{ saleYmd[rowLabel.dataKey] }}
              </td>
              -->
            </tr>
          </tbody>
        </template>
      </v-data-table>
    </v-content>
  </v-app>
</template>

<script>
export default {
  name: 'App',
  data: () => ({
    rowLabels: [],
    dataSet: {},
    headers: [
      { text: '', customClass: 'header-col sticky' },
      { text: '', customClass: 'header-col second sticky' }
    ],
    memInd: '80',
    memIndName: 'D멤버스'
  }),
  methods: {
    fnGetDayHeader (saleYmd) {
      const day = this.$moment(saleYmd, 'YYYYMMDD').format('ddd')
      return {
        customClass: 'header-col',
        saleYmd,
        text: this.$moment(saleYmd, 'YYYYMMDD').format('MM/DD'),
        day,
        color: day === 'Sat' ? 'blue--text darken-2--text' : day === 'Sun' ? 'red--text darken-2--text' : ''
      }
    },
    fnSetHeaders () {
      for (const saleYmd in this.dataSet) {
        this.headers.push(this.fnGetDayHeader(saleYmd))
      }
    },
    fnSetData () {
      this.dataSet = {
        20200501: { data1: 'data1', data2: 'data2', data3: 'data3', data4: 'data4', data5: 'data5', data6: 'data6' },
        20200502: { data1: 'data1', data2: 'data2', data3: 'data3', data4: 'data4', data5: 'data5', data6: 'data6' },
        20200503: { data1: 'data1', data2: 'data2', data3: 'data3', data4: 'data4', data5: 'data5', data6: 'data6' },
        20200504: { data1: 'data1', data2: 'data2', data3: 'data3', data4: 'data4', data5: 'data5', data6: 'data6' },
        20200505: { data1: 'data1', data2: 'data2', data3: 'data3', data4: 'data4', data5: 'data5', data6: 'data6' },
        20200506: { data1: 'data1', data2: 'data2', data3: 'data3', data4: 'data4', data5: 'data5', data6: 'data6' },
        20200507: { data1: 'data1', data2: 'data2', data3: 'data3', data4: 'data4', data5: 'data5', data6: 'data6' },
        20200508: { data1: 'data1', data2: 'data2', data3: 'data3', data4: 'data4', data5: 'data5', data6: 'data6' },
        20200509: { data1: 'data1', data2: 'data2', data3: 'data3', data4: 'data4', data5: 'data5', data6: 'data6' },
        20200510: { data1: 'data1', data2: 'data2', data3: 'data3', data4: 'data4', data5: 'data5', data6: 'data6' },
        20200511: { data1: 'data1', data2: 'data2', data3: 'data3', data4: 'data4', data5: 'data5', data6: 'data6' },
        20200512: { data1: 'data1', data2: 'data2', data3: 'data3', data4: 'data4', data5: 'data5', data6: 'data6' },
        20200513: { data1: 'data1', data2: 'data2', data3: 'data3', data4: 'data4', data5: 'data5', data6: 'data6' },
        20200514: { data1: 'data1', data2: 'data2', data3: 'data3', data4: 'data4', data5: 'data5', data6: 'data6' },
        20200515: { data1: 'data1', data2: 'data2', data3: 'data3', data4: 'data4', data5: 'data5', data6: 'data6' },
        20200516: { data1: 'data1', data2: 'data2', data3: 'data3', data4: 'data4', data5: 'data5', data6: 'data6' },
        20200517: { data1: 'data1', data2: 'data2', data3: 'data3', data4: 'data4', data5: 'data5', data6: 'data6' },
        20200518: { data1: 'data1', data2: 'data2', data3: 'data3', data4: 'data4', data5: 'data5', data6: 'data6' },
        20200519: { data1: 'data1', data2: 'data2', data3: 'data3', data4: 'data4', data5: 'data5', data6: 'data6' },
        20200520: { data1: 'data1', data2: 'data2', data3: 'data3', data4: 'data4', data5: 'data5', data6: 'data6' },
        20200521: { data1: 'data1', data2: 'data2', data3: 'data3', data4: 'data4', data5: 'data5', data6: 'data6' },
        20200522: { data1: 'data1', data2: 'data2', data3: 'data3', data4: 'data4', data5: 'data5', data6: 'data6' },
        20200523: { data1: 'data1', data2: 'data2', data3: 'data3', data4: 'data4', data5: 'data5', data6: 'data6' },
        20200524: { data1: 'data1', data2: 'data2', data3: 'data3', data4: 'data4', data5: 'data5', data6: 'data6' },
        20200525: { data1: 'data1', data2: 'data2', data3: 'data3', data4: 'data4', data5: 'data5', data6: 'data6' },
        20200526: { data1: 'data1', data2: 'data2', data3: 'data3', data4: 'data4', data5: 'data5', data6: 'data6' },
        20200527: { data1: 'data1', data2: 'data2', data3: 'data3', data4: 'data4', data5: 'data5', data6: 'data6' },
        20200528: { data1: 'data1', data2: 'data2', data3: 'data3', data4: 'data4', data5: 'data5', data6: 'data6' },
        20200529: { data1: 'data1', data2: 'data2', data3: 'data3', data4: 'data4', data5: 'data5', data6: 'data6' },
        20200530: { data1: 'data1', data2: 'data2', data3: 'data3', data4: 'data4', data5: 'data5', data6: 'data6' },
        20200531: { data1: 'data1', data2: 'data2', data3: 'data3', data4: 'data4', data5: 'data5', data6: 'data6' },
        20200601: { data1: 'data1', data2: 'data2', data3: 'data3', data4: 'data4', data5: 'data5', data6: 'data6' },
        20200602: { data1: 'data1', data2: 'data2', data3: 'data3', data4: 'data4', data5: 'data5', data6: 'data6' },
        20200603: { data1: 'data1', data2: 'data2', data3: 'data3', data4: 'data4', data5: 'data5', data6: 'data6' },
        20200604: { data1: 'data1', data2: 'data2', data3: 'data3', data4: 'data4', data5: 'data5', data6: 'data6' },
        20200605: { data1: 'data1', data2: 'data2', data3: 'data3', data4: 'data4', data5: 'data5', data6: 'data6' }
      }
      this.rowLabels = [
        {
          colspan: 1,
          rowspan: 2,
          text: '예약률',
          child: { text: '전체', dataKey: 'data1' }
        },
        {
          colspan: 1,
          rowspan: 1,
          text: '기본',
          dataKey: 'data2'
        },
        {
          colspan: 1,
          rowspan: 3,
          isChip: true,
          chipText: this.memInd,
          text: `<br>${this.memIndName}`,
          child: { text: '기준금액', dataKey: 'data3' }
        },
        {
          colspan: 1,
          rowspan: 1,
          text: '현재금액',
          dataKey: 'data4'
        },
        {
          colspan: 1,
          rowspan: 1,
          text: '레벨적용',
          dataKey: 'data5'
        },
        {
          colspan: 2,
          rowspan: 1,
          text: '재고량 전체',
          dataKey: 'data6'
        }
      ]
    }
  },
  created () {
    this.fnSetData()
    this.fnSetHeaders()
  }
}
</script>

<style scope>
  .v-data-table table {
    table-layout: fixed;
  }
  .custom-col {
    width: 100px;
    text-align: center;
    border-right: 1px solid #ccc;
    padding: 0 10px;
  }
  .header-col {
    background-color: #f1f1f1;
    width: 100px;
    border-right: 1px solid #ccc;
    text-align: center !important;
  }
  .header-col.sticky {
    position: sticky;
    left: 0;
    top: auto;
  }
  .header-col.second.sticky {
    left: 100px;
  }
</style>
