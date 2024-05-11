<script setup>
import BoardColumn from "~/components/BoardColumn.vue";
import { useBoardStore } from "~/stores/boardStore";

const boardStore = useBoardStore();
const route = useRoute();
const router = useRouter();

const isModalOpen = computed(() => {
  return route.name === "index-tasks-id";
});

function closeModal() {
  router.push("/");
}

const newColumnName = ref("");

function addColumn() {
  boardStore.addColumn(newColumnName.value);
  newColumnName.value = "";
}
</script>

<template>
  <ClientOnly>
    <div class="board-wrapper">
      <main class="board">
        <BoardColumn
          v-for="(column, columnIndex) in boardStore.board.columns"
          :key="column.id"
          :column="column"
          :columnIndex="columnIndex"
        />
        <UContainer class="column">
          <UInput
            v-model="newColumnName"
            type="text"
            placeholder="Create new column"
            icon="i-heroicons-plus-circle-solid"
            @keydown.enter="addColumn"
          />
        </UContainer>
      </main>
      <div v-show="isModalOpen" class="task-bg" @click.self="closeModal">
        <NuxtPage :key="route.fullPath" />
      </div>
    </div>
  </ClientOnly>
</template>
