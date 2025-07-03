<script lang="ts">
  import { Plus, Minus, Users, Activity, Info } from 'lucide-svelte';

  export let familyMemberCount = 1;

  async function handleSearch({ searchString }: { searchString: string }) {
    return [
      { code: '1', value: `Diabetes`, terminology: 'SNOMED-CT' },
      { code: '2', value: `Hypertension`, terminology: 'SNOMED-CT' },
      { code: '3', value: `Asthma`, terminology: 'SNOMED-CT' },
    ];
  }
</script>

<!-- Problem Diagnosis -->
<div class="bg-white shadow-md rounded-xl p-6 border border-gray-100 mb-6">
  <mb-search
    handleSearch={handleSearch}
    path="gynaec_case_record_sidharth.v0/past_history/problem_diagnosis_name"
    class="w-full mb-4"
  ></mb-search>
  <mb-context
    bind={{ value: "Past", code: "at0061", terminology: "local" }}
    path="gynaec_case_record_sidharth.v0/past_history/problem_diagnosis_qualifier/current_past"
  ></mb-context>
  <div class="hidden">
    <mb-context path="gynaec_case_record_sidharth.v0/past_history/language">  </mb-context>
    <mb-context path="gynaec_case_record_sidharth.v0/past_history/encoding">  </mb-context>
    <mb-context path="gynaec_case_record_sidharth.v0/past_history/subject" ></mb-context>
  </div>
</div>

<!-- Family History -->
<div class="bg-white shadow-md rounded-xl p-6 border border-gray-100">
  <h2 class="text-2xl font-bold text-blue-700 mb-6">👪 Family History</h2>

  <div class="mb-6">
    <!-- svelte-ignore a11y_label_has_associated_control -->
    <!-- svelte-ignore element_invalid_self_closing_tag -->
    <mb-input
      textarea
      class="w-full"
      path="gynaec_case_record_sidharth.v0/family_history/summary"
      label="Summary"
    />
  </div>

  {#each Array(familyMemberCount) as _, index}
    <div class="border border-gray-200 rounded-lg bg-gray-50 shadow-sm p-4 mb-4">
      <h3 class="text-md font-semibold text-gray-700 mb-4">Family Member #{index + 1}</h3>
      <div class="grid grid-cols-1 md:grid-cols-2 gap-4 items-end">
        <!-- Relationship -->
        <div>
          <label class="text-sm font-medium text-gray-700 flex items-center gap-1 mb-1">
            <Users class="w-4 h-4 text-blue-500" />
            Relationship
            <span title="Specify family relationship">
              <Info class="w-3 h-3 text-gray-400 ml-1" />
            </span>
          </label>
          <mb-text-select
            path={`gynaec_case_record_sidharth.v0/family_history/per_family_member:${index}/relationship`}
            class="w-full"
          >
            <!-- svelte-ignore element_invalid_self_closing_tag -->
            <mb-option value="father" label="Father" />
            <!-- svelte-ignore element_invalid_self_closing_tag -->
            <mb-option value="mother" label="Mother" />
            <!-- svelte-ignore element_invalid_self_closing_tag -->
            <mb-option value="sibling" label="Sibling" />
            <!-- svelte-ignore element_invalid_self_closing_tag -->
            <mb-option value="other" label="Other" />
          </mb-text-select>
        </div>

        <!-- Problem Diagnosis -->
        <div>
          <label class="text-sm font-medium text-gray-700 flex items-center gap-1 mb-1">
            <Activity class="w-4 h-4 text-blue-500" />
            Problem / Diagnosis
            <span title="SNOMED-coded problem">
              <Info class="w-3 h-3 text-gray-400 ml-1" />
            </span>
          </label>
          <!-- svelte-ignore element_invalid_self_closing_tag -->
          <mb-search
            handleSearch={handleSearch}
            path={`gynaec_case_record_sidharth.v0/family_history/per_family_member:${index}/clinical_history:0/problem_diagnosis_name`}
            class="w-full"
          />
        </div>
      </div>
    </div>
  {/each}

  <!-- Add/Remove Buttons -->
  <div class="flex justify-start space-x-3 mt-4">
    <button
      on:click={() => familyMemberCount++}
      class="inline-flex items-center px-4 py-2 text-sm font-medium rounded-md border border-blue-500 text-blue-600 hover:bg-blue-50 transition"
    >
      <Plus class="w-4 h-4 mr-1" /> Add Family Member
    </button>
    {#if familyMemberCount > 1}
      <button
        on:click={() => familyMemberCount--}
        class="inline-flex items-center px-4 py-2 text-sm font-medium rounded-md border border-red-400 text-red-600 hover:bg-red-50 transition"
      >
        <Minus class="w-4 h-4 mr-1" /> Remove
      </button>
    {/if}
  </div>
</div>

<!-- Hidden Contexts -->
<div class="hidden">
  <!-- svelte-ignore element_invalid_self_closing_tag -->
  <mb-context path="gynaec_case_record_sidharth.v0/family_history/language" />
  <!-- svelte-ignore element_invalid_self_closing_tag -->
  <mb-context path="gynaec_case_record_sidharth.v0/family_history/encoding" />
  <!-- svelte-ignore element_invalid_self_closing_tag -->
  <mb-context path="gynaec_case_record_sidharth.v0/family_history/subject" />
</div>
