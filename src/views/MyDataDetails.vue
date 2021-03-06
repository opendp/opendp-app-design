<template>
  <div class="my-data-details mt-10">
    <v-container>
      <v-sheet rounded="lg">
        <v-container>
          <h1 class="title-size-2">{{ datasetTitle }}</h1>
          <StatusTag class="my-5" :status="status" />
          <ColoredBorderAlert type="warning" v-if="$vuetify.breakpoint.xsOnly">
            <template v-slot:content>
              If you want to start or continue the process you have to
              <strong>use the desktop version of the app.</strong>
            </template>
          </ColoredBorderAlert>
          <ColoredBorderAlert type="error" v-if="permissionsError">
            <template v-slot:content>
              Check the file permissions in
              <a href="" class="black--text">Dataverse</a> to continue with the
              process.
            </template>
          </ColoredBorderAlert>
          <ColoredBorderAlert type="error" v-if="generalError">
            <template v-slot:content>
              {{ generalErrorSummary }}
            </template>
          </ColoredBorderAlert>
          <ColoredBorderAlert type="warning" v-if="status === IN_EXECUTION">
            <template v-slot:content>
              If canceling, this action cannot be undone.
            </template>
          </ColoredBorderAlert>
          <div class="mb-5" v-if="status === COMPLETED">
            <p class="primary--text">Download DP Release:</p>
            <Button
              :click="handlePDFDownload"
              color="primary"
              classes="d-block mb-2"
            >
              <v-icon left>mdi-download</v-icon><span>PDF file</span>
            </Button>
            <Button
              :click="handleJSONDownload"
              color="primary"
              classes="d-block mb-2"
            >
              <v-icon left>mdi-download</v-icon><span>JSON file</span>
            </Button>
            <Button
              :click="handleHTMLDownload"
              color="primary"
              classes="d-block mb-2"
            >
              <v-icon left>mdi-download</v-icon><span>HTML file</span>
            </Button>
            <Button
              :click="handleXMLDownload"
              color="primary"
              classes="d-block mb-2"
            >
              <v-icon left>mdi-download</v-icon><span>XML file</span>
            </Button>
            <a class="text-decoration-none d-block mt-10" href=""
              >Check DP release in Dataverse
              <v-icon small color="primary">mdi-open-in-new</v-icon></a
            >
          </div>
          <div class="pt-5 pb-10">
            <div v-for="(detail, index) in datasetDetails" :key="index">
              <v-row class="py-3">
                <v-col
                  cols="12"
                  sm="4"
                  class="grey--text text--darken-2 d-flex"
                  :class="{
                    'py-0': $vuetify.breakpoint.xsOnly
                  }"
                >
                  <span>
                    {{ detail.label }}
                  </span>
                  <QuestionIconTooltip :text="detail.tooltip" />
                </v-col>
                <v-col
                  cols="12"
                  sm="8"
                  :class="{
                    'pt-0 pb-5': $vuetify.breakpoint.xsOnly
                  }"
                  >{{ detail.value }}</v-col
                >
              </v-row>
              <v-divider class="hidden-xs-only" />
            </div>
          </div>

          <Button
            v-for="(action, index) in statusInformation[
              status
            ].availableActions.filter(action => action !== VIEW_DETAILS)"
            :key="action + '-' + index"
            color="primary"
            :click="() => handleButtonClick(action, datasetTitle)"
            :label="actionsInformation[action]"
            :class="{
              'width80 mx-auto d-block mb-2': $vuetify.breakpoint.xsOnly,
              'mr-2': $vuetify.breakpoint.smAndUp
            }"
            :disabled="
              action === CONTINUE_WORKFLOW && $vuetify.breakpoint.xsOnly
            "
          />
          <Button
            color="primary"
            outlined
            :click="() => $router.push(NETWORK_CONSTANTS.MY_DATA.PATH)"
            label="Back to My Data"
            :class="{
              'width80 mx-auto d-block': $vuetify.breakpoint.xsOnly
            }"
          />
        </v-container>
      </v-sheet>
    </v-container>
    <SupportBanner />
  </div>
</template>

<script>
import statusInformation from "../data/statusInformation";
import actionsInformation from "../data/actionsInformation";
import QuestionIconTooltip from "../components/DynamicHelpResources/QuestionIconTooltip.vue";
import ColoredBorderAlert from "../components/DynamicHelpResources/ColoredBorderAlert.vue";
import StatusTag from "../components/DesignSystem/StatusTag.vue";
import Button from "../components/DesignSystem/Button.vue";
import SupportBanner from "../components/SupportBanner.vue";
import NETWORK_CONSTANTS from "../router/NETWORK_CONSTANTS";

const {
  IN_PROGRESS,
  IN_EXECUTION,
  ERROR,
  COMPLETED
} = statusInformation.statuses;

const {
  VIEW_DETAILS,
  CONTINUE_WORKFLOW,
  CANCEL_EXECUTION
} = actionsInformation.actions;

export default {
  name: "MyDataDetails",
  components: {
    QuestionIconTooltip,
    ColoredBorderAlert,
    StatusTag,
    Button,
    SupportBanner
  },
  methods: {
    handleButtonClick(action, item) {
      this[action](item);
    },
    continueWorkflow(item) {
      //TODO: Implement Handler
      alert("continue workflow " + item);
    },
    cancelExecution(item) {
      //TODO: Implement Handler
      alert("cancel execution " + item);
    },
    handlePDFDownload() {
      //TODO: Implement Handler
      alert("PDF download!");
    },
    handleJSONDownload() {
      //TODO: Implement Handler
      alert("JSON download!");
    },
    handleHTMLDownload() {
      //TODO: Implement Handler
      alert("HTML download!");
    },
    handleXMLDownload() {
      //TODO: Implement Handler
      alert("XML download!");
    }
  },
  computed: {
    status: function() {
      const urlParam = this.$router.history.current.params.id;
      switch (urlParam) {
        case "exampleInProgress":
          return IN_PROGRESS;
        case "exampleInExecution":
          return IN_EXECUTION;
        case "exampleError":
          return ERROR;
        case "exampleCompleted":
          return COMPLETED;
        default:
          return COMPLETED;
      }
    },
    generalError: function() {
      return this.status === ERROR;
    },
    permissionsError: function() {
      return this.status === IN_PROGRESS;
    }
  },
  data: () => ({
    IN_PROGRESS,
    IN_EXECUTION,
    ERROR,
    COMPLETED,
    VIEW_DETAILS,
    CONTINUE_WORKFLOW,
    CANCEL_EXECUTION,
    statusInformation,
    actionsInformation,
    datasetTitle: "California Demographic Dataset",
    generalErrorSummary: "Error summary: lorem ipsum dolor sit amet.",
    datasetDetails: [
      {
        label: "DV Installation",
        tooltip: "Lorem ipsum dolor sit amet, consectetur adipiscing elit.",
        value: "Lorem ipsum dolor sit amet, consectetur adipiscing elit."
      },
      {
        label: "Remaining time to complete release",
        tooltip: "Lorem ipsum dolor sit amet.",
        value: "13h"
      },
      {
        label: "DV File ID / DOI",
        tooltip: "Lorem ipsum dolor sit amet, consectetur adipiscing elit.",
        value: "Lorem ipsum dolor sit amet, consectetur adipiscing elit."
      },
      {
        label: "Last state in Workflow",
        tooltip: "Lorem ipsum dolor sit amet, consectetur adipiscing elit.",
        value: "Lorem ipsum dolor sit amet, consectetur adipiscing elit."
      },
      {
        label: "Citation",
        tooltip: "Lorem ipsum dolor sit amet, consectetur adipiscing elit.",
        value: "Lorem ipsum dolor sit amet, consectetur adipiscing elit."
      },
      {
        label: "Email address to send notification",
        tooltip: "Lorem ipsum dolor sit amet, consectetur adipiscing elit.",
        value: "example@gmail.com"
      }
      //TODO: The objects above are just examples
      //Here we should use the mounted function of Vue to fill these objects with the real data from the actual dataset
    ],
    NETWORK_CONSTANTS
  })
};
</script>
