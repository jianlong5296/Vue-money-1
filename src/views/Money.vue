<template>
    <Layout class-prefix="layout">
        <NumberPad :value.sync="record.amount" @submit="saveRecord"/>
        <Types :value.sync="record.value"/>
        <div class="notes">
            <FormItem field-name="备注"
                      placeholder="在这里输入备注"
                      @update:value="onUpdateNotes"
            />
        </div>
        <Tags/>
    </Layout>
</template>

<script lang="ts">
    import Vue from 'vue';
    import NumberPad from '@/components/Money/NumberPad.vue';
    import Types from '@/components/Money/Types.vue';
    import FormItem from '@/components/Money/FormItem.vue';
    import Tags from '@/components/Money/Tags.vue';
    import {Component} from 'vue-property-decorator';

    @Component({
        components: {Tags, FormItem, Types, NumberPad},
    })
    export default class Money extends Vue {
        get recordList(){
            return this.$store.state.recordList;
        }
        created() {
            this.$store.commit('fetchRecords');

        }

        onUpdateAmount(value: string) {
            this.record.amount = parseFloat(value);
        }

        onUpdateType(value: string) {
            this.record.type = value;
        }

        onUpdateNotes(value: string) {
            this.record.notes = value;
        }

        saveRecord() {
            this.$store.commit('createRecord', this.record);
        }
    }
</script>

<style lang="scss">
    .layout-content {
        display: flex;
        flex-direction: column-reverse;
    }

    .notes {
        padding: 12px 0;
    }
</style>
