<template>
  <section class="Dashboard">
    <v-row v-if="user.roleId !== 1 && user.roleId !== 2">
      <v-col cols="12" md="6" xl="3">
        <v-card hover link to="projetos">
          <v-card-text>
            <div>Informativo</div>
            <p class="text-h4">
              Projetos ativos
            </p>
            <span class="text-h2 text--primary">
              {{ projects }}
            </span>
          </v-card-text>
        </v-card>
      </v-col>
      <v-col cols="12" md="6" xl="3">
        <v-card hover link to="atividades">
          <v-card-text>
            <div>Informativo</div>
            <p class="text-h4">
              Suas tarefas
            </p>
            <span class="text-h2 text--primary">
              {{ tasks }}
            </span>
          </v-card-text>
        </v-card>
      </v-col>
      <v-col cols="12" md="6" xl="3">
        <v-card hover link to="atividades">
          <v-card-text>
            <div>Informativo</div>
            <p class="text-h4">
              Seus bugs
            </p>
            <span class="text-h2 text--primary">
              {{ bugs }}
            </span>
          </v-card-text>
        </v-card>
      </v-col>
      <v-col cols="12">
        <DashboardProjetosAtivos />
      </v-col>
      <v-col cols="12">
        <DashboardEmployeeActivity />
      </v-col>
    </v-row>

    <v-row v-else>
      <v-col cols="12">
        <Subheader title="Dashboard" />
      </v-col>
      <v-col cols="12" xl="6">
        <DashboardProjetosAtivos />
      </v-col>
      <v-col cols="12" xl="6">
        <DashboardProjetosHorasRegistradas />
      </v-col>
      <v-col cols="12" xl="6">
        <DashboardUsuarioHorasRegistradas />
      </v-col>
      <v-col cols="12" xl="6">
        <DashboardEmployeeActivity />
      </v-col>
    </v-row>
  </section>
</template>

<script>
import Project from "./models/Project";
import Task from "./models/Task";
import { mapGetters } from "vuex";
import DashboardProjetosAtivos from "./components/DashboardProjetosAtivos.vue";
import DashboardProjetosHorasRegistradas from "./components/DashboardProjetosHorasRegistradas.vue";
import DashboardUsuarioHorasRegistradas from "./components/DashboardUsuarioHorasRegistradas.vue";
import DashboardEmployeeActivity from "./components/DashboardEmployeeActivity.vue";

export default {
  name: "Dashboard",
  components: {
    DashboardProjetosAtivos,
    DashboardProjetosHorasRegistradas,
    DashboardUsuarioHorasRegistradas,
    DashboardEmployeeActivity
  },
  computed: {
    ...mapGetters("User", {
      user: "getUser"
    })
  },
  data() {
    return {
      projects: 0,
      tasks: 0,
      bugs: 0,
      finishedTasks: 0
    };
  },
  async created() {
    this.projects = await new Project().countProjects();
    this.tasks = await new Task().countTasks();
    this.bugs = await new Task().countBugs();
  },
  methods: {}
};
</script>

<style lang="scss" scoped>
// .Dashboard {
//   &__Card {
//     position: relative;
//     height: 200px;

//     h2 {
//       font-size: 30px;
//     }

//     span {
//       position: absolute;
//       color: var(--v-text-base);
//       opacity: 0.8;
//       bottom: 0;
//       right: 0;
//       font-size: 160px;
//       align-items: baseline;
//       vertical-align: text-bottom;
//       line-height: 120px;
//     }

//     &:hover {
//       span {
//         opacity: 1;
//       }
//     }
//   }
// }
</style>
