<template>
  <v-container class="d-flex flex-column h-screen">
    <div class="pa-4 d-flex flex-row align-content-center align-center">
      <RoomPicker
        v-model:roomid="roomid"
        variant="underlined"
        hide-details
      ></RoomPicker>
      <v-dialog v-model="showDialog" persistent max-width="768px">
        <template v-slot:activator="{ props }">
          <v-btn v-bind="props" append-icon="mdi-filter" variant="flat">
            {{ $props.functionalButtonText ?? "更多" }}
          </v-btn>
          <v-btn
            color="primary"
            append-icon="mdi-magnify"
            variant="flat"
            @click="() => emit('query')"
            >搜索</v-btn
          >
        </template>
        <v-card>
          <v-card-title>
            <span class="text-h5">查询条件</span>
          </v-card-title>
          <v-card-text>
            <slot name="form" :roomid="roomid"> </slot>
          </v-card-text>
          <v-card-actions>
            <v-spacer></v-spacer>
            <v-btn
              color="blue-darken-1"
              variant="text"
              @click="showDialog = false"
            >
              确定
            </v-btn>
          </v-card-actions>
        </v-card>
      </v-dialog>
    </div>

    <v-card class="flex-grow-1 flex-shrink-1">
      <slot name="content"></slot>
    </v-card>
  </v-container>
</template>
<script lang="ts" setup>
import { ref, watch } from "vue";
import RoomPicker from "../input/RoomPicker.vue";

export type Props = {
  functionalButtonText?: string;
  roomid: number;
};
export type Emits = {
  (event: "query"): void;
  (event: "update:roomid", roomid: number): void;
};
const emit = defineEmits<Emits>();
const props = defineProps<Props>();
const showDialog = ref(false);
const roomid = ref(props.roomid);
watch(roomid, (newRoomid) => {
  emit("update:roomid", newRoomid);
});
</script>
