<template>
    <mdb-container>
    <mdb-row>
    <mdb-col col='4'>Left Column</mdb-col>
    <mdb-col col='4'>Medium Column</mdb-col>
    <mdb-col col='4'>Right Column</mdb-col>
    
    </mdb-row>
    </mdb-container>
</template>

<script>
import Event from '@/components/Event';
import { mdbContainer, mdbRow, mdbCol, mdbIcon } from "mdbvue";
export default {
    name : "Grid",
    components : {
        mdbContainer,
        mdbRow,
        mdbCol,
        Event,
    }
}
</script>

<style>
div[class^="col"],
div[class*=" col"] {
  border: 1px solid black;
}
</style>
