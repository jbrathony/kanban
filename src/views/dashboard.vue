<template>
  <div class="container-fluid">
    <div class="row">
      <div class="col">
        <h4 class="my-3">Active Boards</h4>
      </div>
    </div>
    <div class="row equal">
      <div class="col-xs-12 col-sm-6 col-md-3 d-flex pb-3" v-for="board in unarchivedBoards" :key="board.id" v-if="board.id != undefined">

        <div class="card w-100 board-item" >
          <div class="card-body">
            <div class="d-flex justify-content-between">
              <h5 class="card-title flex-nowrap">{{ board.name }}</h5>
              <span @click="handleTaskBoardEditing(board)">...</span>
            </div>
            <p class="card-text">{{ board.description }}</p>
          </div>
          <div class="card-footer bg-transparent">
            <router-link class="btn btn-sm btn-app mr-2 mb-1" :to="{ name: 'task-board', params: { id: board.id } }"
              >Open board</router-link>
            <!-- <button class="btn btn-sm btn-danger mb-1" @click="handleArchiveTaskBoard(board)">
              Archive
            </button> -->
          </div>
        </div>
      </div>
    </div>
    <div class="row" v-show="this.archivedBoards.length > 0">
      <div class="col">
        <h4 class="my-3">Archived Boards</h4>
      </div>
    </div>
    <div class="row equal" v-show="this.archivedBoards.length > 0">
      <div class="col-xs-12 col-sm-6 col-md-3 d-flex pb-3" v-for="board in archivedBoards" :key="board.id">
        <div class="card w-100 board-item">
          <div class="card-body">
            <h5 class="card-title flex-nowrap">{{ board.name }}</h5>
            <p class="card-text">{{ board.description }}</p>
          </div>
          <div class="card-footer bg-transparent text-center">
            <button class="btn btn-sm btn-success" @click="handleRestoreTaskBoard(board)">
              Restore
            </button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
import { mapGetters, mapActions } from "vuex"
import { Bus } from "@/utils/bus"
export default {
  computed: {
    ...mapGetters({
      unarchivedBoards: "unarchivedBoards",
      archivedBoards: "archivedBoards"
    })
  },
  methods: {
    ...mapActions({
      setActiveTaskBoard: "setActiveTaskBoard",
      archiveTaskBoard: "archiveTaskBoard",
      restoreTaskBoard: "restoreTaskBoard"
    }),
    handleTaskBoardEditing(board) {
      Bus.$emit("taskboard-editing", board)
    },
    handleArchiveTaskBoard(board) {
      this.archiveTaskBoard({ boardId: board.id })
    },
    handleRestoreTaskBoard(board) {
      this.restoreTaskBoard({ boardId: board.id })
    }
  },
  async created() {
    await this.setActiveTaskBoard({
      board: null
    })
  }
}
</script>
