# Create an Aggregation Pipeline

The Aggregation Pipeline Builder in Atlas helps you create [aggregation pipelines](/core/aggregation-pipeline/) to process documents from a collection or view and return computed results.

## About this Task

Atlas provides different modes to create aggregation pipelines:

- Stage View Mode, a visual pipeline editor that preloads pipeline syntax based on your selected stages.
- Stage Wizard, a feature of Stage View Mode that provides a set of templates for simple aggregation stage use cases. The Stage Wizard only includes simple use cases to help you get started with your aggregation pipeline.
- Focus Mode, a feature of Stage View Mode where you edit one pipeline stage at a time. Focus Mode helps you manage complex or deeply nested aggregation pipeline stages.
- Text View Mode, a text-based pipeline editor that accepts raw pipeline syntax.

## Before You Begin

To build an aggregation pipeline, choose a collection and click the **Aggregations** tab. Atlas displays a blank aggregation pipeline. The **Preview of Documents in the Collection** section shows 10 documents randomly sampled from the chosen collection.

> **Note:**
> When you connect to a MongoDB deployment hosted on [Atlas](https://www.mongodb.com/cloud/atlas), Atlas-only stages [$search](/reference/atlas-search/query-syntax/#-search) and [$searchMeta](/reference/atlas-search/query-syntax/#-searchmeta) become available in the Aggregation Pipeline Builder. Use these stages to perform [full-text search](/atlas-search/atlas-search-overview) on Atlas collections.

## Steps

To see how to create an aggregation pipeline, select the tab corresponding to your chosen view mode:

**Stage View Mode**

1. In Atlas, go to the **Data Explorer** page for your project.

   1. If it's not already displayed, select the organization that contains your project from the *[icon: office]* **Organizations** menu in the navigation bar.
   2. If it's not already displayed, select your project from the **Projects** menu in the navigation bar.
   3. In the sidebar, click **Data Explorer** under the **Database** heading. The [Data Explorer](https://cloud.mongodb.com/go?l=https%3A%2F%2Fcloud.mongodb.com%2Fv2%2F%3Cproject%3E%23%2Fmetrics%2FreplicaSet%2F%3Creplset%3E%2Fexplorer) displays.

IMPORTANT: You can also go to the **Clusters** page, and click **Data Explorer** under the **Shortcuts** heading.

1. Select the **Stages** view

   1. Select the collection.
   2. In the aggregation pipeline pane, ensure the **{} Stages** toggle switch is selected.

   ![Toggle on stage view mode](/images/atlas-ui/compass/agg-builder-stageview-toggle-on.png)

1. **Add an aggregation stage**

   At the bottom of the aggregation pipeline pane, click the **+ Add Stage** button.

1. **Select an aggregation pipeline stage**

   On the upper-left corner of the aggregation stage card, click the **Select** drop-down menu and select the [aggregation pipeline stage](/reference/operator/aggregation-pipeline/) to use for the first stage of the pipeline.

1. **Fill in your pipeline stage**

   Fill in your selected stage. You can adjust the width of the pipeline stage by dragging its border to the right.

   > **Note:**
   > The toggle to the right of each pipeline stage name dictates whether that stage is included in the pipeline. Toggling a pipeline stage also updates the pipeline preview, which updates based on whether or not that stage is included.
   >
   > For example, the following pipeline excludes the first [$match](/reference/operator/aggregation/match/) stage and only includes the [$project](/reference/operator/aggregation/project/) stage:
   >
   > ![Aggregation Builder exclude stage example](/images/atlas-ui/compass/agg-builder-exclude-stage-example.png)
   >

1. **Add additional pipeline stages**

   To add an additional pipeline stage after your last aggregation stage, click **Add Stage**. To add an aggregation stage before your most recently added stage, click the plus **+** icon above the stage card. Repeat steps 3 and 4 for each additional stage.

   > **Note:**
   > You can change the order of pipeline stages by dragging the header of each stage card.

1. **Run the pipeline**

   At the top-right corner of the pipeline builder, click **Run**. Atlas returns your results in the document view.

   > **Warning:**
   > Some aggregation operators, like `$merge` and `$out`, can modify your collection's data. If your aggregation pipeline contains operators that can modify your collection's data, you are prompted for confirmation before the pipeline is executed.
   >

---

**Stage Wizard**

1. In Atlas, go to the **Data Explorer** page for your project.

   1. If it's not already displayed, select the organization that contains your project from the *[icon: office]* **Organizations** menu in the navigation bar.
   2. If it's not already displayed, select your project from the **Projects** menu in the navigation bar.
   3. In the sidebar, click **Data Explorer** under the **Database** heading. The [Data Explorer](https://cloud.mongodb.com/go?l=https%3A%2F%2Fcloud.mongodb.com%2Fv2%2F%3Cproject%3E%23%2Fmetrics%2FreplicaSet%2F%3Creplset%3E%2Fexplorer) displays.

IMPORTANT: You can also go to the **Clusters** page, and click **Data Explorer** under the **Shortcuts** heading.

1. Select the **Stages** view

   1. Select the collection.
   2. In the aggregation pipeline pane, ensure the **{} Stages** toggle switch is selected.

   ![Toggle on stage view mode](/images/atlas-ui/compass/agg-builder-stageview-toggle-on.png)

1. Open the **Stage Wizard** card

   To the right of the view mode toggle, click the wand icon to open the **Stage Wizard** card.

   ![Click the icon to the right of the view mode toggle.](/images/atlas-ui/compass/agg-builder-stage-wizard.png)

1. **(Optional) Search for an aggregation use case**

   On the **Stage Wizard** card, you can filter the use cases by searching for keywords associated with the use case or aggregation stage.

1. **Select an aggregation pipeline stage use case**

   On the **Stage Wizard** card, select a stage use case for the first stage of your pipeline. You can click the stage card to add it to the end of your pipeline or drag it to your preferred position. After you select a use case, Atlas populates the stage card with a form that corresponds to the selected aggregation pipeline stage. The Stage Wizard use cases include the following aggregation stages:

   - `$group`
   - `$lookup`
   - `$match`
   - `$project`
   - `$sort`

1. **Fill in your pipeline stage**

   Fill in the form for your selected stage and click **Apply**. After you click **Apply**, the form will turn into a stage card that you can edit in Stage View Mode, Focus Mode, or Text View Mode. Atlas populates the Stage Output with up to ten sample output documents.

   > **Note:**
   > You cannot edit an existing stage through the Stage Wizard. The Stage Wizard can only add new stages. To edit an existing stage, use Stage View Mode, Focus Mode, or Text View Mode.

1. **Add additional pipeline stages**

   To add more aggregation stages to your pipeline, repeat steps 3 and 4 for each additional stage.

   > **Tip:**
   > You can change the order of pipeline stages by dragging the header of each stage card.

1. **Run the pipeline**

   At the top-right corner of the pipeline builder, click **Run**. Atlas returns your results in the document view.

---

**Focus Mode**

1. In Atlas, go to the **Data Explorer** page for your project.

   1. If it's not already displayed, select the organization that contains your project from the *[icon: office]* **Organizations** menu in the navigation bar.
   2. If it's not already displayed, select your project from the **Projects** menu in the navigation bar.
   3. In the sidebar, click **Data Explorer** under the **Database** heading. The [Data Explorer](https://cloud.mongodb.com/go?l=https%3A%2F%2Fcloud.mongodb.com%2Fv2%2F%3Cproject%3E%23%2Fmetrics%2FreplicaSet%2F%3Creplset%3E%2Fexplorer) displays.

IMPORTANT: You can also go to the **Clusters** page, and click **Data Explorer** under the **Shortcuts** heading.

1. Select the **Stages** view

   1. Select the collection.
   2. In the aggregation pipeline pane, ensure the

   **{} Stages** toggle switch is selected.

   ![Toggle on stage view mode](/images/atlas-ui/compass/agg-builder-stageview-toggle-on.png)

1. **Add an aggregation stage**

   If you have not already created an aggregation stage, click the **+ Add Stage** button at the bottom of the aggregation pipeline pane.

1. **Open Focus Mode**

   On the upper-right corner of the stage card, click the Focus Mode icon.

   ![Select the Focus Mode button](/images/atlas-ui/compass/focus-mode-button.png)

1. **Select an aggregation pipeline stage**

   Click the **Select** drop-down menu and select the [aggregation pipeline stage](/reference/operator/aggregation-pipeline/) to use for the first stage of the pipeline.

1. **Fill in your pipeline stage**

   Fill in your selected  stage. Atlas populates the **Stage Output** with up to ten sample output documents. You can adjust the width of the **Stage Input**, stage editor, and the **Stage Output** by dragging their border to the desired size.

   > **Note:**
   > The toggle to the right of each pipeline stage name dictates whether that stage is included in the pipeline. Toggling a pipeline stage also updates the pipeline preview, which updates based on whether or not that stage is included.
   >

1. **Add additional pipeline stages**

   Click the **Add Stage** dropdown to add additional aggregation stages before or after your last aggregation stage. Repeat steps 4 and 5 for each additional stage. You can add stages with the following keyboard shortcuts:

   - To add a stage after the current stage:

      - Windows / Linux: **Ctrl + Shift + A**
      - Mac: **⌘ + Shift + A**
   - To add a stage before the current stage:

      - Windows / Linux: **Ctrl + Shift + B**
      - Mac: **⌘ + Shift + B**

1. **Navigate between stages**

   To navigate between different stages, select the stage you want to edit from the **Stage** dropdown in the upper-left corner of the Focus Mode modal. You can navigate between stages with the following keyboard shortcuts:

   - To go to the stage before the current stage:

      - Windows / Linux: **Ctrl + Shift + 9**
      - Mac: **⌘ + Shift + 9**
   - To add a stage before the current stage:

      - Windows / Linux: **Ctrl + Shift + 0**
      - Mac: **⌘ + Shift + 0**

1. **Run the pipeline**

   Click **x** to exit Focus Mode and select **Run** at the top right of the pipeline builder. Atlas returns your results in the document view.

---

**Text View Mode**

1. In Atlas, go to the **Data Explorer** page for your project.

   1. If it's not already displayed, select the organization that contains your project from the *[icon: office]* **Organizations** menu in the navigation bar.
   2. If it's not already displayed, select your project from the **Projects** menu in the navigation bar.
   3. In the sidebar, click **Data Explorer** under the **Database** heading. The [Data Explorer](https://cloud.mongodb.com/go?l=https%3A%2F%2Fcloud.mongodb.com%2Fv2%2F%3Cproject%3E%23%2Fmetrics%2FreplicaSet%2F%3Creplset%3E%2Fexplorer) displays.

IMPORTANT: You can also go to the **Clusters** page, and click **Data Explorer** under the **Shortcuts** heading.

1. Select the **Text** view

   1. Select the collection.
   2. In the aggregation pipeline pane, click the **</> Text** toggle switch to enable text mode for pipeline editing.

   ![Toggle textmode on](/images/atlas-ui/compass/agg-builder-textview-toggle-on.png)

1. **Enter your aggregation pipeline**

   Enter valid aggregation syntax into the text editor. The text editor provides real-time linting for correct syntax and debugging information. You can also use Text View Mode to import aggregation pipelines from plain text by typing or pasting your pipeline into the text editor. For example, following pipeline limits the query results to `4` documents.

   ```javascript
   [ { "$limit" : 4 } ]
   ```

   > **Note:**
   > To expand all embedded fields and documents within the preview results, click **Output Options** and select **Expand all fields**.
   >
   > ![Pipeline Output preview](/images/atlas-ui/compass/agg-builder-textview-expand-pipeline-preview.png)
   >

1. **Run the pipeline**

   Click **Run** at the top right of the pipeline builder. Atlas returns your results in the document view.

---

## Learn More

- [Aggregation Pipeline](/core/aggregation-pipeline/)
- [Aggregation Pipeline Stages](/reference/operator/aggregation-pipeline/)
- atlas-ui-pipeline-builder-settings
- atlas-ui-export-pipeline

- [Save a Pipeline](/atlas-ui/agg-pipeline-builder/save-agg-pipeline)
- [Open a Pipeline](/atlas-ui/agg-pipeline-builder/open-saved-pipeline)
- [View Explain Plans](/atlas-ui/agg-pipeline-builder/view-pipeline-explain-plan)
- [Export to a Language](/atlas-ui/agg-pipeline-builder/export-pipeline-to-language)
- [Create a View](/atlas-ui/agg-pipeline-builder/create-a-view)
- [Count Results](/atlas-ui/agg-pipeline-builder/count-pipeline-results)
- [Specify Collation](/atlas-ui/agg-pipeline-builder/pipeline-custom-collation)
- [Set Max Time MS](/atlas-ui/agg-pipeline-builder/maxtime-ms-pipeline)
- [Builder Settings](/atlas-ui/agg-pipeline-builder/aggregation-pipeline-builder-settings)
