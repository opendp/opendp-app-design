<template>
  <div class="wizard-page">
    <v-container>
      <v-row>
        <v-col>
          <v-stepper v-model="stepperPosition" id="wizard-content" alt-labels>
            <StepperHeader :steps="steps" :stepperPosition="stepperPosition" />
            <v-stepper-items>
              <span class="d-block mt-5"
                >Used dataset:
                <!-- TODO: This name should be the original from the loaded dataset -->
                <a href="http://" class="text-decoration-none"
                  >California Demographic Dataset
                  <v-icon small color="primary">mdi-open-in-new</v-icon></a
                ></span
              >
              <v-stepper-content :complete="stepperPosition > 0" step="0">
                <ValidateDataset v-on:stepCompleted="updateStepStatus" />
              </v-stepper-content>
              <v-stepper-content :complete="stepperPosition > 1" step="1">
                <ConfirmVariables v-on:stepCompleted="updateStepStatus" />
              </v-stepper-content>
              <v-stepper-content :complete="stepperPosition > 2" step="2">
                <SetEpsilonValue v-on:stepCompleted="updateStepStatus" />
              </v-stepper-content>
              <v-stepper-content :complete="stepperPosition > 3" step="3">
                <CreateStatistics v-on:stepCompleted="updateStepStatus" />
              </v-stepper-content>
              <v-stepper-content :complete="stepperPosition > 4" step="4">
                <GenerateDPRelease v-on:stepCompleted="updateStepStatus" />
              </v-stepper-content>
            </v-stepper-items>
          </v-stepper>
          <WizardNavigationButtons
            :steps="steps"
            :stepperPosition.sync="stepperPosition"
            class="hidden-md-and-up"
          />
        </v-col>
        <v-col cols="3" lg="2" class="hidden-sm-and-down">
          <WizardNavigationButtons
            :steps="steps"
            :stepperPosition.sync="stepperPosition"
          />
        </v-col>
      </v-row>
    </v-container>
  </div>
</template>

<style scoped lang="scss">
.v-stepper {
  box-shadow: none;
}

.v-stepper__content {
  padding-left: 0;
}
</style>

<style lang="scss">
.v-stepper__wrapper {
  padding: 1px;
}
</style>

<script>
import ConfirmVariables from "@/views/WizardSteps/ConfirmVariables";
import SetEpsilonValue from "./WizardSteps/SetEpsilonValue.vue";
import CreateStatistics from "./WizardSteps/CreateStatistics.vue";
import GenerateDPRelease from "./WizardSteps/GenerateDPRelease.vue";
import StepperHeader from "../components/Wizard/StepperHeader.vue";
import WizardNavigationButtons from "../components/Wizard/WizardNavigationButtons.vue";
import ValidateDataset from "./WizardSteps/ValidateDataset.vue";
export default {
  name: "Wizard",
  components: {
    ConfirmVariables,
    SetEpsilonValue,
    CreateStatistics,
    GenerateDPRelease,
    StepperHeader,
    WizardNavigationButtons,
    ValidateDataset
  },
  methods: {
    updateStepStatus: function(stepNumber, completedStatus) {
      this.steps[stepNumber].completed = completedStatus;
    }
  },

  data: () => ({
    stepperPosition: 0,
    steps: [
      {
        title: "Validate Dataset",
        completed: false
      },
      {
        title: "Confirm Variables",
        completed: false
      },
      {
        title: "Set Epsilon Value",
        completed: false
      },
      {
        title: "Create Statistics",
        completed: false
      },
      {
        title: "Generate DP",
        completed: true
      }
    ]
  })
};
</script>
