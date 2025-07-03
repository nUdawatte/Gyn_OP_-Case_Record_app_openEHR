<script lang="ts">
  import {
    Stethoscope,
    Droplet,
    Thermometer,
    Gauge,
    ChevronDown,
    ChevronRight,
    Info
  } from 'lucide-svelte';

  let showGeneral = true;
  let showUrinalysis = true;
  let showBP = true;
</script>

<!-- 🩺 General / Abdomen / Pelvic Exams -->
<!-- svelte-ignore a11y_click_events_have_key_events -->
<!-- svelte-ignore a11y_no_static_element_interactions -->
<div class="bg-white shadow-md rounded-xl p-6 border border-gray-100 mb-6">
  <!-- svelte-ignore a11y_click_events_have_key_events -->
  <div class="flex justify-between items-center cursor-pointer" on:click={() => showGeneral = !showGeneral}>
    <h2 class="text-2xl font-bold text-blue-700 flex items-center gap-2">
      <Stethoscope class="w-6 h-6" />
      Physical
    </h2>
    {#if showGeneral}
      <ChevronDown class="w-5 h-5 text-gray-500" />
    {:else}
      <ChevronRight class="w-5 h-5 text-gray-500" />
    {/if}
  </div>

  {#if showGeneral}
    <div class="mt-4 flex flex-col gap-4">
      <mb-input textarea path="gynaec_case_record_sidharth.v0/physical_examination_findings/general/clinical_description" label="General examination"></mb-input>
      <mb-input textarea path="gynaec_case_record_sidharth.v0/physical_examination_findings/abdomen/clinical_description" label="Abdomen examination" ></mb-input>
      <mb-input textarea path="gynaec_case_record_sidharth.v0/physical_examination_findings/pelvic/clinical_description" label="Pelvic examination"></mb-input>
    </div>
  {/if}
</div>

<!-- 💧 Urinalysis -->
<!-- svelte-ignore a11y_click_events_have_key_events -->
<!-- svelte-ignore a11y_no_static_element_interactions -->
<div class="bg-white shadow-md rounded-xl p-6 border border-gray-100 mb-6">
  <!-- svelte-ignore a11y_click_events_have_key_events -->
  <div class="flex justify-between items-center cursor-pointer" on:click={() => showUrinalysis = !showUrinalysis}>
    <h2 class="text-2xl font-bold text-indigo-700 flex items-center gap-2">
      <Droplet class="w-6 h-6" />
      Urinalysis
    </h2>
    {#if showUrinalysis}
      <ChevronDown class="w-5 h-5 text-gray-500" />
    {:else}
      <ChevronRight class="w-5 h-5 text-gray-500" />
    {/if}
  </div>

  {#if showUrinalysis}
    <div class="grid grid-cols-1 md:grid-cols-3 gap-4 mt-4">
      <mb-select path="gynaec_case_record_sidharth.v0/urinalysis/point_in_time:0/glucose" label="Glucose">
        <mb-option value="at0115" label="Negative"></mb-option>
        <mb-option value="at0116" label="Trace"></mb-option>
        <mb-option value="at0117" label="1+"></mb-option>
        <mb-option value="at0118" label="2+"></mb-option>
        <mb-option value="at0119" label="3+"></mb-option>
        <mb-option value="at0120" label="4+"></mb-option>
      </mb-select>
      <mb-select path="gynaec_case_record_sidharth.v0/urinalysis/point_in_time:0/ketones" label="Ketones">
        <mb-option value="at0109" label="Negative"></mb-option>
        <mb-option value="at0110" label="Trace"></mb-option>
        <mb-option value="at0111" label="Small"></mb-option>
        <mb-option value="at0112" label="Moderate"></mb-option>
        <mb-option value="at0113" label="Large"></mb-option>
        <mb-option value="at0114" label="Large+"></mb-option>
      </mb-select>
      <mb-select path="gynaec_case_record_sidharth.v0/urinalysis/point_in_time:0/blood" label="Blood">
        <mb-option value="at0102" label="Negative"></mb-option>
        <mb-option value="at0103" label="Non-haemolysed Trace"></mb-option>
        <mb-option value="at0104" label="Non-haemolysed Moderate"></mb-option>
        <mb-option value="at0105" label="Haemolysed Trace"></mb-option>
        <mb-option value="at0106" label="1+"></mb-option>
        <mb-option value="at0107" label="2+"></mb-option>
        <mb-option value="at0108" label="3+"></mb-option>
      </mb-select>
      <mb-select path="gynaec_case_record_sidharth.v0/urinalysis/point_in_time:0/ph" label="pH">
        <mb-option value="at0127" label="5.0"></mb-option>
        <mb-option value="at0128" label="5.5"></mb-option>
        <mb-option value="at0129" label="6.0"></mb-option>
        <mb-option value="at0130" label="6.5"></mb-option>
        <mb-option value="at0131" label="7.0"></mb-option>
        <mb-option value="at0132" label="7.5"></mb-option>
        <mb-option value="at0133" label="8.0"></mb-option>
        <mb-option value="at0134" label="8.5"></mb-option>
        <mb-option value="at0176" label="9.0"></mb-option>
        <mb-option value="at0177" label="9.5"></mb-option>
        <mb-option value="at0179" label="10.0"></mb-option>
      </mb-select>
      <mb-select path="gynaec_case_record_sidharth.v0/urinalysis/point_in_time:0/protein" label="Protein">
        <mb-option value="at0096" label="Negative"></mb-option>
        <mb-option value="at0097" label="Trace"></mb-option>
        <mb-option value="at0098" label="1+"></mb-option>
        <mb-option value="at0099" label="2+"></mb-option>
        <mb-option value="at0100" label="3+"></mb-option>
        <mb-option value="at0101" label="4+"></mb-option>
      </mb-select>
    </div>
  {/if}
</div>

<!-- 🩸 Blood Pressure -->
<!-- svelte-ignore a11y_click_events_have_key_events -->
<!-- svelte-ignore a11y_no_static_element_interactions -->
<div class="bg-white shadow-md rounded-xl p-6 border border-gray-100 mb-6">
  <!-- svelte-ignore a11y_click_events_have_key_events -->
  <div class="flex justify-between items-center cursor-pointer" on:click={() => showBP = !showBP}>
    <h2 class="text-2xl font-bold text-red-700 flex items-center gap-2">
      <Gauge class="w-6 h-6" />
      Blood Pressure
    </h2>
    {#if showBP}
      <ChevronDown class="w-5 h-5 text-gray-500" />
    {:else}
      <ChevronRight class="w-5 h-5 text-gray-500" />
    {/if}
  </div>

  {#if showBP}
    <div class="flex items-center mt-4 space-x-3">
      <mb-quantity default="mm[Hg]" hideunit path="gynaec_case_record_sidharth.v0/blood_pressure/systolic" class="w-28">
        <mb-unit unit="mm[Hg]" label="mm[Hg]"></mb-unit>
      </mb-quantity>
      <span class="text-2xl font-bold text-gray-500">/</span>
      <mb-quantity default="mm[Hg]" hideunit path="gynaec_case_record_sidharth.v0/blood_pressure/diastolic" class="w-28">
        <mb-unit unit="mm[Hg]" label="mm[Hg]"></mb-unit>
      </mb-quantity>
      <span class="text-sm text-gray-500 ml-2">mm Hg</span>
    </div>
  {/if}
</div>

<!-- 🧬 Contexts -->
<div class="hidden">
  <mb-context path="gynaec_case_record_sidharth.v0/blood_pressure/time"></mb-context>
  <mb-context path="gynaec_case_record_sidharth.v0/blood_pressure/language"></mb-context>
  <mb-context path="gynaec_case_record_sidharth.v0/blood_pressure/encoding"></mb-context>
  <mb-context path="gynaec_case_record_sidharth.v0/blood_pressure/subject"></mb-context>

  <mb-context path="gynaec_case_record_sidharth.v0/urinalysis/point_in_time:0/time"></mb-context>
  <mb-context path="gynaec_case_record_sidharth.v0/urinalysis/language"></mb-context>
  <mb-context path="gynaec_case_record_sidharth.v0/urinalysis/encoding"></mb-context>
  <mb-context path="gynaec_case_record_sidharth.v0/urinalysis/subject"></mb-context>

  <mb-context path="gynaec_case_record_sidharth.v0/physical_examination_findings/general/system_or_structure_examined" bind={"General"}></mb-context>
  <mb-context path="gynaec_case_record_sidharth.v0/physical_examination_findings/abdomen/system_or_structure_examined" bind={"Abdomen"}></mb-context>
  <mb-context path="gynaec_case_record_sidharth.v0/physical_examination_findings/pelvic/system_or_structure_examined" bind={"Pelvic"}></mb-context>
  <mb-context path="gynaec_case_record_sidharth.v0/physical_examination_findings/time"></mb-context>
  <mb-context path="gynaec_case_record_sidharth.v0/physical_examination_findings/language"></mb-context>
  <mb-context path="gynaec_case_record_sidharth.v0/physical_examination_findings/encoding"></mb-context>
  <mb-context path="gynaec_case_record_sidharth.v0/physical_examination_findings/subject"></mb-context>
</div>
