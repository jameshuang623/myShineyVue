<template>
  <Layout class-prefix="layout">
    {{ recordList }}
    <Tags :data-source.sync="tags" :value.sync="record.tags"/>
    <Notes :value.sync="record.notes"/>
    <Types :value.sync="record.type"/>
    <NumberPad :value.sync="record.amount" @submit="saveRecord"/>
  </Layout>
</template>

<script lang="ts">
import NumberPad from '@/components/Money/NumberPad.vue';
import Types from '@/components/Money/Types.vue';
import Notes from '@/components/Money/Notes.vue';
import Tags from '@/components/Money/Tags.vue';
import Vue from 'vue';
import {Component, Watch} from 'vue-property-decorator';
import model from '@/model';

const recordList = model.fetch();


@Component({
  components: {Tags, Notes, Types, NumberPad}
})
export default class Money extends Vue {
  tags = ['衣', '食', '住', '行', '彩'];
  recordList: RecordItem[] = JSON.parse(window.localStorage.getItem('recordList') || '[]');
  record: RecordItem = {
    tags: [], notes: '', type: '-', amount: 0
  };

  // onUpdateTags(value: string[]) {
  //   this.record.tags = value
  // }

  // onUpdateNotes(value: string) {
  //   this.record.notes = value
  // }

  saveRecord() {
    const record2: RecordItem = model.clone(this.record);
    record2.createdAt = new Date();
    this.recordList.push(record2);
  }

  @Watch('recordList')
  onRecordChange() {
    model.save(this.recordList)
  }
}
</script>


<style lang="scss">
.layout-content {
  display: flex;
  flex-direction: column;
}
</style>

<style lang="scss" scoped>
</style>