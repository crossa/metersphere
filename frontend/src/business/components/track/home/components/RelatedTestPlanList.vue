<template>
  <home-base-component :title="$t('test_track.home.my_plan')" v-loading>
    <el-table
      :data="tableData"
      @row-click="intoPlan"
      v-loading="result.loading">
      <el-table-column
        prop="name"
        fixed
        :label="$t('commons.name')"
        show-overflow-tooltip>
      </el-table-column>

      <el-table-column
        prop="status"
        :label="$t('test_track.plan.plan_status')"
        show-overflow-tooltip>
        <template v-slot:default="scope">
          <plan-status-table-item :value="scope.row.status"/>
        </template>
      </el-table-column>

      <el-table-column
        prop="projectName"
        :label="$t('test_track.pass_rate')"
        show-overflow-tooltip>
        <template v-slot:default="scope">
          {{scope.row.passRate}}%
        </template>
      </el-table-column>

      <el-table-column
        prop="projectName"
        :label="$t('test_track.home.tested_case')"
        show-overflow-tooltip>
        <template v-slot:default="scope">
          {{scope.row.tested}}/{{scope.row.total}}
        </template>
      </el-table-column>

      <el-table-column
        prop="projectName"
        :label="$t('test_track.home.test_rate')"
        min-width="100"
        show-overflow-tooltip>
        <template v-slot:default="scope">
          <el-progress :percentage="scope.row.testRate"></el-progress>
        </template>
      </el-table-column>

      <el-table-column
        prop="stage"
        :label="$t('test_track.plan.plan_stage')"
        show-overflow-tooltip>
        <template v-slot:default="scope">
          <plan-stage-table-item :stage="scope.row.stage"/>
        </template>
      </el-table-column>
      <el-table-column
        prop="projectName"
        :label="$t('test_track.plan.plan_project')"
        show-overflow-tooltip>
      </el-table-column>

    </el-table>

  </home-base-component>
</template>

<script>
    import HomeBaseComponent from "./HomeBaseComponent";
    import PlanStatusTableItem from "../../common/tableItems/plan/PlanStatusTableItem";
    import PlanStageTableItem from "../../common/tableItems/plan/PlanStageTableItem";
    import MsTableOperator from "../../../common/components/MsTableOperator";
    export default {
      name: "RelatedTestPlanList",
      components: {MsTableOperator, PlanStageTableItem, PlanStatusTableItem, HomeBaseComponent},
      data() {
        return {
          result: {},
          tableData: []
        }
      },
      mounted() {
        this.initTableData();
      },
      methods: {
        initTableData() {
          this.result = this.$post('/test/plan/list/all/relate', this.condition, response => {
            this.tableData = response.data;
          });
        },
        intoPlan(row, event, column) {
          this.$router.push('/track/plan/view/' + row.id);
        }
      }
    }
</script>

<style scoped>
</style>
