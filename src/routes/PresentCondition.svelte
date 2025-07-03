<script lang="ts">
  import { Info, Plus, Minus, CalendarDays, Timer, Activity } from "lucide-svelte";
  import { onMount } from "svelte";

  export let symptomCount = 1;
  let openCards: boolean[] = [];

  onMount(() => {
    openCards = Array(symptomCount).fill(true);
  });

  function toggleCard(index: number) {
    openCards[index] = !openCards[index];
  }

  async function handleSearch({ searchString }: { searchString: string }) {
    return [
      { code: '1', value: `Symptom ${searchString} 1`, terminology: 'SNOMED-CT' },
      { code: '2', value: `Symptom ${searchString} 2`, terminology: 'SNOMED-CT' },
      { code: '3', value: `Symptom ${searchString} 3`, terminology: 'SNOMED-CT' }
    ];
  }

  $: if (symptomCount !== openCards.length) {
    openCards = Array(symptomCount).fill(true);
  }
</script>

<div class="bg-white shadow-md rounded-xl p-6 border border-gray-100">

    <div class="mb-6">
    <p class="text-gray-600 mb-2">Describe the patient's current condition:</p>
    <mb-input textarea path="gynaec_case_record_sidharth.v0/story_history/present_condition:0"></mb-input>
    </div>
    <!-- svelte-ignore a11y_label_has_associated_control -->
      
    <h2 class="text-2xl font-bold text-blue-700 mb-6">🩺 Signs and Symptoms</h2>

  <div class="space-y-4">
    {#each Array(symptomCount) as _, index}
      <div class="border border-gray-200 rounded-lg bg-gray-50 shadow-sm transition-all">
        <!-- Collapsible Header -->
        <!-- svelte-ignore a11y_click_events_have_key_events -->
        <!-- svelte-ignore a11y_no_static_element_interactions -->
        <div class="flex items-center justify-between p-4 cursor-pointer" on:click={() => toggleCard(index)}>
          <div class="font-semibold text-gray-700">Symptom #{index + 1}</div>
          <button class="text-sm text-blue-600 hover:underline">
            {openCards[index] ? "Collapse" : "Expand"}
          </button>
        </div>

        {#if openCards[index]}
          <div class="px-4 pb-4">
            <div class="grid grid-cols-1 md:grid-cols-3 gap-4 items-end">
              <!-- Symptom/Sign -->
              <div>
                <label class="text-sm font-medium text-gray-700 flex items-center gap-1 mb-1">
                  <Activity class="w-4 h-4 text-blue-500" />
                  Symptom / Sign
                  <span title="Select symptom name using SNOMED search">
                    <Info class="w-3 h-3 text-gray-400 ml-1" />
                  </span>
                </label>
                <!-- svelte-ignore element_invalid_self_closing_tag -->
                <mb-search
                  handleSearch={handleSearch}
                  path={`gynaec_case_record_sidharth.v0/story_history/symptom_sign:${index}/symptom_sign_name`}
                  class="w-full"
                />
              </div>

              <!-- Episode Onset -->
              <div>
                <label class="text-sm font-medium text-gray-700 flex items-center gap-1 mb-1">
                  <CalendarDays class="w-4 h-4 text-blue-500" />
                  Episode Onset
                  <span title="Select date when this symptom started">
                    <Info class="w-3 h-3 text-gray-400 ml-1" />
                  </span>
                </label>
                <!-- svelte-ignore element_invalid_self_closing_tag -->
                <mb-date
                  path={`gynaec_case_record_sidharth.v0/story_history/symptom_sign:${index}/episode_onset`}
                  class="w-full"
                />
              </div>

              <!-- Episode Duration -->
              <div>
                <label class="text-sm font-medium text-gray-700 flex items-center gap-1 mb-1">
                  <Timer class="w-4 h-4 text-blue-500" />
                  Episode Duration
                  <span title="Specify duration in days">
                    <Info class="w-3 h-3 text-gray-400 ml-1" />
                  </span>
                </label>
                <!-- svelte-ignore element_invalid_self_closing_tag -->
                <mb-duration
                  day
                  path={`gynaec_case_record_sidharth.v0/story_history/symptom_sign:${index}/episode_duration`}
                  class="w-full"
                />
              </div>
            </div>
          </div>
        {/if}
      </div>
    {/each}
  </div>

  <!-- Add / Remove Buttons -->
  <div class="flex justify-start space-x-3 mt-6">
    <button
      on:click={() => symptomCount++}
      class="inline-flex items-center px-4 py-2 text-sm font-medium rounded-md border border-blue-500 text-blue-600 hover:bg-blue-50 transition"
    >
      <Plus class="w-4 h-4 mr-1" /> Add Symptom
    </button>
    {#if symptomCount > 1}
      <button
        on:click={() => symptomCount--}
        class="inline-flex items-center px-4 py-2 text-sm font-medium rounded-md border border-red-400 text-red-600 hover:bg-red-50 transition"
      >
        <Minus class="w-4 h-4 mr-1" /> Remove
      </button>
    {/if}
  </div>
</div>

<!-- Context Fields -->
<div class="hidden">
  <!-- svelte-ignore element_invalid_self_closing_tag -->
  <mb-context path="gynaec_case_record_sidharth.v0/story_history/time" />
  <!-- svelte-ignore element_invalid_self_closing_tag -->
  <mb-context path="gynaec_case_record_sidharth.v0/story_history/language" />
  <!-- svelte-ignore element_invalid_self_closing_tag -->
  <mb-context path="gynaec_case_record_sidharth.v0/story_history/encoding" />
  <!-- svelte-ignore element_invalid_self_closing_tag -->
  <mb-context path="gynaec_case_record_sidharth.v0/story_history/subject" />
</div>
