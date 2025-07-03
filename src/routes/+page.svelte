<script lang="ts">
  import "medblocks-ui";
  import "medblocks-ui/dist/styles";

  import { onMount } from 'svelte';

  import PresentCondition from "./PresentCondition.svelte";
  import PastHistory from "./PastHistory.svelte";
  import SexualHistory from "./SexualHistory.svelte";
  import Examination from "./Examination.svelte";
  import DiagnosisRecommendations from "./DiagnosisRecommendations.svelte";

  import { count } from "medblocks-ui/dist/utils";
  import { toast } from "svelte-sonner";
  import {
    HeartPulse,
    ClipboardList,
    Venus,
    Stethoscope,
    FileSignature
  } from "lucide-svelte";
  type CompositionRecord = {
    time: string;
    uid: string;
  };

  let compositions: CompositionRecord[] = [];

  let selectedComposition: any;
  let currentStep = 0;
  let form: any;

  let ehrId = "f9e7debe-070a-438d-ad2d-bfbfcc7e0c4c";
  let templateId = "gynaec_case_record_sidharth.v0";

  let symptomCount = 1;
  let familyMemberCount = 1;
  let medicationOrderCount = 1;
  let serviceRequestCount = 1;
  let provisionalDiagnosisCount = 1;

  const sections = [
    { label: "Present Condition", icon: HeartPulse },
    { label: "Past History", icon: ClipboardList },
    { label: "Sexual History", icon: Venus },
    { label: "Examination", icon: Stethoscope },
    { label: "Diagnosis & Recommendations", icon: FileSignature }
  ];

  onMount(async () => {
  compositions = await listCompositions();
  });

  const handleEdit = async (uid: string) => {
    toast.loading("Loading composition...");
    try {
      const response = await fetch(
        `https://openehr-bootcamp.medblocks.com/ehrbase/rest/openehr/v1/ehr/${ehrId}/composition/${uid}`,
        {
          method: "GET",
          headers: {
            Accept: "application/openehr.wt.flat.schema+json"
          }
        }
      );
      const data = await response.json();
      selectedComposition = data;
      symptomCount = count(data, `${templateId}/story_history/symptom_sign`);
      familyMemberCount = count(data, `${templateId}/family_history/per_family_member`);
      medicationOrderCount = count(data, `${templateId}/medication_order/order`);
      serviceRequestCount = count(data, `${templateId}/service_request/current_activity`);
      provisionalDiagnosisCount = count(data, `${templateId}/problem_diagnosis`);
      form.import(data);
      toast.success("Composition loaded successfully");
    } catch (error) {
      toast.error("Failed to load composition");
    }
  };

  const handleSubmit = async () => {
    const composition = form.export();
    toast.loading("Submitting form...");
    try {
      const response = await fetch(
        `https://openehr-bootcamp.medblocks.com/ehrbase/rest/openehr/v1/ehr/${ehrId}/composition?templateId=${templateId}`,
        {
          method: "POST",
          headers: {
            Accept: "application/openehr.wt.flat.schema+json",
            "Content-Type": "application/openehr.wt.flat.schema+json",
            Prefer: "return=representation"
          },
          body: JSON.stringify(composition)
        }
      );
      const data = await response.json();
      toast.success("Form submitted successfully");
      console.log(data);
    } catch (error) {
      toast.error("Submission failed");
    }
  };

  const listCompositions = async (): Promise<CompositionRecord[]> => {
    const response = await fetch(
      `https://openehr-bootcamp.medblocks.com/ehrbase/rest/openehr/v1/query/aql`,
      {
        method: "POST",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify({
          q: `SELECT c/context/start_time/value as time, c/uid/value as uid FROM EHR e CONTAINS COMPOSITION c WHERE e/ehr_id/value = '${ehrId}' AND c/archetype_details/template_id/value = '${templateId}' ORDER BY c/context/start_time/value`
        })
      }
    );
    const data = await response.json();
    return data.rows.map((row: any) => ({
      time: row[0],
      uid: row[1]
    }));
  };


  const deleteComposition = async (uid: string) => {
    const confirmDelete = confirm("Are you sure you want to delete this composition?");
    if (!confirmDelete) return;

    toast.loading("Deleting composition...");
    try {
      await fetch(
        `https://openehr-bootcamp.medblocks.com/ehrbase/rest/openehr/v1/ehr/${ehrId}/composition/${uid}`,
        { method: "DELETE" }
      );
      toast.success("Composition deleted successfully");
      compositions = await listCompositions(); // ✅ Refresh list
    } catch (error) {
      toast.error("Failed to delete composition");
    }
  };

</script>

<!-- svelte-ignore css_unused_selector -->
<style>
  .icon-tab svg {
    width: 20px;
    height: 20px;
    margin-right: 0.5rem;
  }
</style>

<div class="w-full min-h-screen bg-gray-50 p-6">
  <h1 class="text-3xl font-bold text-center text-blue-800 mb-8">
    🩺 Gynaec OP Case Record
  </h1>

  <div class="grid grid-cols-1 lg:grid-cols-12 gap-6">
    <!-- Sidebar -->
    <aside class="lg:col-span-2 bg-white rounded-xl shadow-md p-4">
      <ul class="space-y-2">
        {#each sections as { label, icon: Icon }, index}
          <li>
            <button
              class={`w-full flex items-center p-3 rounded-lg text-left transition-all duration-200 ${
                currentStep === index
                  ? "bg-blue-100 text-blue-700 font-semibold"
                  : "hover:bg-gray-100 text-gray-700"
              }`}
              on:click={() => (currentStep = index)}
            >
              <Icon class="w-5 h-5 mr-2" />
              {label}
            </button>
          </li>
        {/each}
      </ul>
    </aside>

    <!-- Form -->
    <main class="lg:col-span-6 bg-white rounded-xl shadow-md p-6">
      <mb-form bind:this={form}>
        <!-- svelte-ignore element_invalid_self_closing_tag -->
        <mb-context path="{templateId}/category" />
        <!-- svelte-ignore element_invalid_self_closing_tag -->
        <mb-context path="{templateId}/language" />
        <!-- svelte-ignore element_invalid_self_closing_tag -->
        <mb-context path="{templateId}/territory" />
        <!-- svelte-ignore element_invalid_self_closing_tag -->
        <mb-context path="{templateId}/composer" />
        <!-- svelte-ignore element_invalid_self_closing_tag -->
        <mb-context path="{templateId}/context/setting" />
        <!-- svelte-ignore element_invalid_self_closing_tag -->
        <mb-context path="{templateId}/context/start_time" />

        <div class:hidden={currentStep !== 0}>
          <h2 class="text-xl font-semibold mb-4 text-blue-700 flex items-center gap-2">
            <HeartPulse class="w-5 h-5 text-red-500" />
            Present Condition
          </h2>
          <PresentCondition symptomCount={symptomCount} />
        </div>
        <div class:hidden={currentStep !== 1}>
          <h2 class="text-xl font-semibold mb-4 text-blue-700 flex items-center gap-2">
            <ClipboardList class="w-5 h-5 text-red-500" />
            Past History
          </h2>
          <PastHistory familyMemberCount={familyMemberCount} />
        </div>
        <div class:hidden={currentStep !== 2}>
          <h2 class="text-xl font-semibold mb-4 text-blue-700 flex items-center gap-2">
            <Venus class="w-5 h-5 text-red-500" />
            Sexual History
          </h2>
          <SexualHistory />
        </div>
        <div class:hidden={currentStep !== 3}>
          <h2 class="text-xl font-semibold mb-4 text-blue-700 flex items-center gap-2">
            <Stethoscope class="w-5 h-5 text-red-500" />
            Examination
          </h2>
          <Examination />
        </div>
        <div class:hidden={currentStep !== 4}>
          <h2 class="text-xl font-semibold mb-4 text-blue-700 flex items-center gap-2">
            <FileSignature class="w-5 h-5 text-red-500" />
            Diagnosis & Recommendations
          </h2>
          <DiagnosisRecommendations
            medicationOrderCount={medicationOrderCount}
            serviceRequestCount={serviceRequestCount}
            provisionalDiagnosisCount={provisionalDiagnosisCount}
          />

        </div>
      </mb-form>

      <div class="flex justify-between mt-6">
        <button
          class="px-4 py-2 bg-gray-200 text-gray-700 rounded-lg hover:bg-gray-300 disabled:opacity-50"
          disabled={currentStep === 0}
          on:click={() => currentStep--}
        >
          Previous
        </button>
        {#if currentStep !== 4}
          <button
            class="px-4 py-2 bg-blue-600 text-white rounded-lg hover:bg-blue-700"
            on:click={() => currentStep++}
          >
            Next
          </button>
        {:else}
          <button
            class="px-4 py-2 bg-green-600 text-white rounded-lg hover:bg-green-700"
            on:click={handleSubmit}
          >
            Submit
          </button>
        {/if}
      </div>
    </main>

    <!-- Composition List -->
    <section class="lg:col-span-4 bg-white rounded-xl shadow-md p-6 overflow-auto max-h-[80vh]">
      <h2 class="text-xl font-semibold text-blue-700 mb-4">Previous Records</h2>

      {#if compositions.length === 0}
        <div class="text-gray-500">No records found.</div>
      {:else}
        {#each compositions as composition}
          <div class="p-4 border rounded-lg shadow-sm mb-3 hover:bg-gray-100 transition-colors">
            <div class="font-medium text-gray-800">{composition.time}</div>
            <div class="text-xs text-gray-500 mt-1">UID: {composition.uid}</div>
            <div class="flex mt-2 space-x-2">
              <button
                on:click={() => handleEdit(composition.uid)}
                class="px-3 py-1 text-sm rounded border border-blue-300 text-blue-600 hover:bg-blue-50"
              >
                Edit
              </button>
              <button
                on:click={() => deleteComposition(composition.uid)}
                class="px-3 py-1 text-sm rounded border border-red-300 text-red-600 hover:bg-red-50"
              >
                Delete
              </button>
            </div>
          </div>
        {/each}
      {/if}
    </section>
  </div>
</div>
