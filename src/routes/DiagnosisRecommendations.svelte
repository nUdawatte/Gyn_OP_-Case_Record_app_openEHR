<script lang="ts">
  import { onMount } from 'svelte';
  import { Plus, Minus, Info, ClipboardList, Syringe, Stethoscope } from 'lucide-svelte';

  export let provisionalDiagnosisCount = 1;
  export let medicationOrderCount = 1;
  export let serviceRequestCount = 1;

  let openSections = {
    diagnosis: true,
    finalDiagnosis: true,
    medication: true,
    services: true
  };

  function toggle(section: keyof typeof openSections) {
    openSections[section] = !openSections[section];
  }
</script>

<!-- Provisional Diagnosis -->
<div class="bg-white border border-gray-200 rounded-xl p-6 shadow-sm mt-6">
  <div class="flex justify-between items-center mb-4">
    <h2 class="text-xl font-semibold text-blue-700 flex items-center gap-2">
      <ClipboardList class="w-5 h-5" /> Provisional Diagnosis
    </h2>
    <button on:click={() => toggle('diagnosis')} class="text-sm text-blue-600 hover:underline">
      {openSections.diagnosis ? 'Collapse' : 'Expand'}
    </button>
  </div>

  {#if openSections.diagnosis}
    <div class="space-y-4">
      {#each Array(provisionalDiagnosisCount) as _, index}
        <div class="p-4 bg-gray-50 rounded-lg border border-gray-100">
          <!-- svelte-ignore element_invalid_self_closing_tag -->
          <mb-input path={`gynaec_case_record_sidharth.v0/problem_diagnosis:${index}/problem_diagnosis_name`} label="Diagnosis" />
          <!-- svelte-ignore element_invalid_self_closing_tag -->
          <mb-context bind={{value: 'Working', code: 'at0017', terminology: 'local'}} path={`gynaec_case_record_sidharth.v0/problem_diagnosis:${index}/problem_diagnosis_qualifier/diagnostic_status`} />
          <!-- svelte-ignore element_invalid_self_closing_tag -->
          <mb-context bind={{value: 'Current', code: "at0062", terminology: "local"}} path={`gynaec_case_record_sidharth.v0/problem_diagnosis:${index}/problem_diagnosis_qualifier/current_past`} />
          <!-- svelte-ignore element_invalid_self_closing_tag -->
          <mb-context path={`gynaec_case_record_sidharth.v0/problem_diagnosis:${index}/language`} />
          <!-- svelte-ignore element_invalid_self_closing_tag -->
          <mb-context path={`gynaec_case_record_sidharth.v0/problem_diagnosis:${index}/encoding`} />
          <!-- svelte-ignore element_invalid_self_closing_tag -->
          <mb-context path={`gynaec_case_record_sidharth.v0/problem_diagnosis:${index}/subject`} />
        </div>
      {/each}
    </div>

    <div class="flex gap-3 mt-4">
      <button on:click={() => provisionalDiagnosisCount++} class="px-4 py-1 text-sm border border-blue-500 text-blue-600 rounded hover:bg-blue-50">
        <Plus class="w-4 h-4 inline mr-1" /> Add
      </button>
      {#if provisionalDiagnosisCount > 1}
        <button on:click={() => provisionalDiagnosisCount--} class="px-4 py-1 text-sm border border-red-500 text-red-600 rounded hover:bg-red-50">
          <Minus class="w-4 h-4 inline mr-1" /> Remove
        </button>
      {/if}
    </div>
  {/if}
</div>

<!-- Final Diagnosis -->
<div class="bg-white border border-gray-200 rounded-xl p-6 shadow-sm mt-8">
  <div class="flex justify-between items-center mb-4">
    <h2 class="text-xl font-semibold text-green-700 flex items-center gap-2">
      <ClipboardList class="w-5 h-5" /> Final Diagnosis
    </h2>
    <button on:click={() => toggle('finalDiagnosis')} class="text-sm text-blue-600 hover:underline">
      {openSections.finalDiagnosis ? 'Collapse' : 'Expand'}
    </button>
  </div>

  {#if openSections.finalDiagnosis}
    <mb-input path="gynaec_case_record_sidharth.v0/final_diagnosis/problem_diagnosis_name" label="Final Diagnosis"></mb-input>
    <mb-context bind={{value: 'Established', code: 'at0018', terminology: 'local'}} path="gynaec_case_record_sidharth.v0/final_diagnosis/problem_diagnosis_qualifier/diagnostic_status"></mb-context>
    <mb-context bind={{value: 'Current', code: 'at0062', terminology: 'local'}} path="gynaec_case_record_sidharth.v0/final_diagnosis/problem_diagnosis_qualifier/current_past"></mb-context>
    <mb-context path="gynaec_case_record_sidharth.v0/final_diagnosis/language" ></mb-context>
    <mb-context path="gynaec_case_record_sidharth.v0/final_diagnosis/encoding"></mb-context>
    <mb-context path="gynaec_case_record_sidharth.v0/final_diagnosis/subject"></mb-context>
  {/if}
</div>

<!-- Medication Order -->
<div class="bg-white border border-gray-200 rounded-xl p-6 shadow-sm mt-8">
  <div class="flex justify-between items-center mb-4">
    <h2 class="text-xl font-semibold text-purple-700 flex items-center gap-2">
      <Syringe class="w-5 h-5" /> Medication Order
    </h2>
    <button on:click={() => toggle('medication')} class="text-sm text-blue-600 hover:underline">
      {openSections.medication ? 'Collapse' : 'Expand'}
    </button>
  </div>

  {#if openSections.medication}
    {#each Array(medicationOrderCount) as _, index}
      <div class="grid grid-cols-1 md:grid-cols-4 gap-4 mb-4 p-4 bg-gray-50 rounded-lg border">
        <mb-input path={`gynaec_case_record_sidharth.v0/medication_order/order:${index}/medication_item`} label="Medication"></mb-input>
        <mb-input path={`gynaec_case_record_sidharth.v0/medication_order/order:${index}/overall_directions_description`} label="Directions"></mb-input>
        <mb-date path={`gynaec_case_record_sidharth.v0/medication_order/order:${index}/order_details/order_start_date_time`} label="Start Date"></mb-date>
        <mb-date path={`gynaec_case_record_sidharth.v0/medication_order/order:${index}/order_details/order_stop_date_time`} label="End Date"></mb-date>
        <mb-context path={`gynaec_case_record_sidharth.v0/medication_order/order:${index}/timing`} ></mb-context>
      </div>
    {/each}

    <div class="flex gap-3 mt-2">
      <button on:click={() => medicationOrderCount++} class="px-4 py-1 text-sm border border-blue-500 text-blue-600 rounded hover:bg-blue-50">
        <Plus class="w-4 h-4 inline mr-1" /> Add
      </button>
      {#if medicationOrderCount > 1}
        <button on:click={() => medicationOrderCount--} class="px-4 py-1 text-sm border border-red-500 text-red-600 rounded hover:bg-red-50">
          <Minus class="w-4 h-4 inline mr-1" /> Remove
        </button>
      {/if}
    </div>

    <mb-context path="gynaec_case_record_sidharth.v0/medication_order/narrative"></mb-context>
    <mb-context path="gynaec_case_record_sidharth.v0/medication_order/language" ></mb-context>
    <mb-context path="gynaec_case_record_sidharth.v0/medication_order/encoding"></mb-context>
    <mb-context path="gynaec_case_record_sidharth.v0/medication_order/subject" ></mb-context>
  {/if}
</div>

<!-- Service Requests -->
<div class="bg-white border border-gray-200 rounded-xl p-6 shadow-sm mt-8 mb-6">
  <div class="flex justify-between items-center mb-4">
    <h2 class="text-xl font-semibold text-pink-700 flex items-center gap-2">
      <Stethoscope class="w-5 h-5" /> Service Requests
    </h2>
    <button on:click={() => toggle('services')} class="text-sm text-blue-600 hover:underline">
      {openSections.services ? 'Collapse' : 'Expand'}
    </button>
  </div>

  {#if openSections.services}
    {#each Array(serviceRequestCount) as _, index}
      <div class="grid grid-cols-1 md:grid-cols-3 gap-4 mb-4 p-4 bg-gray-50 rounded-lg border">
        <mb-input path={`gynaec_case_record_sidharth.v0/service_request/current_activity:${index}/service_name`} label="Service Name"></mb-input>
        <mb-input path={`gynaec_case_record_sidharth.v0/service_request/current_activity:${index}/description`} label="Description"></mb-input>
        <mb-date path={`gynaec_case_record_sidharth.v0/service_request/current_activity:${index}/service_due`} label="Service Due" ></mb-date>
        <mb-context path={`gynaec_case_record_sidharth.v0/service_request/current_activity:${index}/timing`} ></mb-context>
      </div>
    {/each}

    <div class="flex gap-3 mt-2">
      <button on:click={() => serviceRequestCount++} class="px-4 py-1 text-sm border border-blue-500 text-blue-600 rounded hover:bg-blue-50">
        <Plus class="w-4 h-4 inline mr-1" /> Add
      </button>
      {#if serviceRequestCount > 1}
        <button on:click={() => serviceRequestCount--} class="px-4 py-1 text-sm border border-red-500 text-red-600 rounded hover:bg-red-50">
          <Minus class="w-4 h-4 inline mr-1" /> Remove
        </button>
      {/if}
    </div>

    <mb-context path="gynaec_case_record_sidharth.v0/service_request/narrative" ></mb-context>
    <mb-context path="gynaec_case_record_sidharth.v0/service_request/language" ></mb-context>
    <mb-context path="gynaec_case_record_sidharth.v0/service_request/encoding" ></mb-context>
    <mb-context path="gynaec_case_record_sidharth.v0/service_request/subject" ></mb-context>
  {/if}
</div>
