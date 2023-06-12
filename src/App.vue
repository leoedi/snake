<template>
  <DxDataGrid :data-source="drivers">
    <DxColumn data-field="busID">
      <DxLookup :data-source="buses" value-expr="busID" display-expr="plates" />
    </DxColumn>
  </DxDataGrid>
</template>

<script>
import "devextreme/dist/css/dx.light.css";

import { DxDataGrid, DxColumn, DxLookup } from "devextreme-vue/data-grid";

const drivers = [
  { driverID: 1, firstName: "John", lastName: "Smith", busID: 2 },
  { driverID: 2, firstName: "Lizzy", lastName: "Cook", busID: 1 },
  { driverID: 3, firstName: "Brian", lastName: "Hawkins", busID: 3 },
  { driverID: 4, firstName: "edris", lastName: "soltani", busID: 4 },
];

const buses = [
  { busID: 1, plates: "123456" },
  { busID: 2, plates: "AB-1234" },
  { busID: 3, plates: "CD-9876" },
  { busID: 4, plates: "CD-98ss76" },
];

export default {
  components: {
    DxDataGrid,
    DxColumn,
    DxLookup,
  },
  data() {
    return {
      drivers,
      buses,
    };
  },
};
</script>
