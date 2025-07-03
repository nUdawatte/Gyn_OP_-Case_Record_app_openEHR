<script lang="ts">
  import {
    Heart,
    Clock,
    Droplets,
    CalendarDays,
    Info,
    ChevronDown,
    ChevronRight
  } from 'lucide-svelte';

  let showSexual = true;
  let showMenstrual = true;
</script>

<!-- 🧡 Sexual Health Summary -->
<!-- svelte-ignore a11y_click_events_have_key_events -->
<div class="bg-white shadow-md rounded-xl p-6 border border-gray-100 mb-6">
  <!-- svelte-ignore a11y_click_events_have_key_events -->
  <!-- svelte-ignore a11y_no_static_element_interactions -->
  <div
    class="flex justify-between items-center cursor-pointer"
    on:click={() => (showSexual = !showSexual)}
  >
    <h2 class="text-2xl font-bold text-pink-700 flex items-center gap-2">
      <Heart class="w-6 h-6" />
      Sexual Health Summary
    </h2>
    {#if showSexual}
      <ChevronDown class="w-5 h-5 text-gray-500" />
    {:else}
      <ChevronRight class="w-5 h-5 text-gray-500" />
    {/if}
  </div>

  {#if showSexual}
    <div class="mt-6 grid grid-cols-1 md:grid-cols-2 gap-4">
      <!-- Status -->
      <div>
        <label class="text-sm font-medium text-gray-700 flex items-center gap-1 mb-1">
          <Heart class="w-4 h-4 text-pink-500" />
          Status
          <span title="Sexual activity status">
            <Info class="w-3 h-3 text-gray-400 ml-1" />
          </span>
        </label>
        <mb-buttons
          path="gynaec_case_record_sidharth.v0/sexual_health_summary/status"
          terminology="local"
          class="w-full"
        >
          <mb-option value="at0009" label="Sexually active, regular partner"></mb-option>
          <mb-option value="at0010" label="Sexually active, no regular partner"></mb-option>
          <mb-option value="at0011" label="Never sexually active"></mb-option>
        </mb-buttons>
      </div>

      <!-- First ever sex -->
      <div>
        <label class="text-sm font-medium text-gray-700 flex items-center gap-1 mb-1">
          <Clock class="w-4 h-4 text-pink-500" />
          First ever sex (years)
          <Info class="w-3 h-3 text-gray-400 ml-1" />
        </label>
        <mb-duration
          year
          path="gynaec_case_record_sidharth.v0/sexual_health_summary/first_ever_sex"
          class="w-full"
        ></mb-duration>
      </div>
    </div>
  {/if}
</div>

<!-- 🌙 Menstruation Summary -->
<!-- svelte-ignore a11y_no_static_element_interactions -->
<div class="bg-white shadow-md rounded-xl p-6 border border-gray-100">
  <!-- svelte-ignore a11y_click_events_have_key_events -->
  <div
    class="flex justify-between items-center cursor-pointer"
    on:click={() => (showMenstrual = !showMenstrual)}
  >
    <h2 class="text-2xl font-bold text-purple-700 flex items-center gap-2">
      <Droplets class="w-6 h-6" />
      Menstruation Summary
    </h2>
    {#if showMenstrual}
      <ChevronDown class="w-5 h-5 text-gray-500" />
    {:else}
      <ChevronRight class="w-5 h-5 text-gray-500" />
    {/if}
  </div>

  {#if showMenstrual}
    <div class="mt-6">
      <!-- Overall description -->
      <div class="mb-4">
        <!-- svelte-ignore a11y_label_has_associated_control -->
        <label class="text-sm font-medium text-gray-700 mb-1 block">
          Overall Description
        </label>
        <mb-input
          textarea
          path="gynaec_case_record_sidharth.v0/menstruation_summary/overall_description"
          class="w-full"
        ></mb-input>
      </div>

      <div class="grid grid-cols-1 md:grid-cols-3 gap-4">
        <!-- Menarche -->
        <div>
          <label class="text-sm font-medium text-gray-700 flex items-center gap-1 mb-1">
            <Clock class="w-4 h-4 text-purple-500" />
            Menarche (years)
            <span title="Age at first menstruation">
              <Info class="w-3 h-3 text-gray-400 ml-1" />
            </span>
          </label>
          <mb-duration
            year
            path="gynaec_case_record_sidharth.v0/menstruation_summary/menarche:0"
            class="w-full"
          ></mb-duration>
        </div>

        <!-- Typical pattern -->
        <div>
          <label class="text-sm font-medium text-gray-700 flex items-center gap-1 mb-1">
            <Droplets class="w-4 h-4 text-purple-500" />
            Typical Pattern
            <Info class="w-3 h-3 text-gray-400 ml-1" />
          </label>
          <mb-buttons
            path="gynaec_case_record_sidharth.v0/menstruation_summary/per_episode/typical_pattern"
            terminology="local"
            class="w-full"
          >
            <mb-option value="at0015" label="Regular" ></mb-option>
            <mb-option value="at0016" label="Irregular" > </mb-option>
          </mb-buttons>
        </div>

        <!-- Date of onset (LMP) -->
        <div>
          <label class="text-sm font-medium text-gray-700 flex items-center gap-1 mb-1">
            <CalendarDays class="w-4 h-4 text-purple-500" />
            Date of Onset (LMP)
            <Info class="w-3 h-3 text-gray-400 ml-1" />
          </label>
          <mb-date
            path="gynaec_case_record_sidharth.v0/last_menstrual_period/date_of_onset_lmp"
            class="w-full"
          > </mb-date>
        </div>
      </div>
    </div>
  {/if}
</div>

<!-- Hidden Context Bindings -->
<div class="hidden">
  <mb-context path="gynaec_case_record_sidharth.v0/sexual_health_summary/language"></mb-context>
  <mb-context path="gynaec_case_record_sidharth.v0/sexual_health_summary/encoding"></mb-context>
  <mb-context path="gynaec_case_record_sidharth.v0/sexual_health_summary/subject"></mb-context>
  <mb-context path="gynaec_case_record_sidharth.v0/menstruation_summary/language"></mb-context>
  <mb-context path="gynaec_case_record_sidharth.v0/menstruation_summary/encoding"></mb-context>
  <mb-context path="gynaec_case_record_sidharth.v0/menstruation_summary/subject"></mb-context>
  <mb-context path="gynaec_case_record_sidharth.v0/last_menstrual_period/language"></mb-context>
  <mb-context path="gynaec_case_record_sidharth.v0/last_menstrual_period/encoding"></mb-context>
  <mb-context path="gynaec_case_record_sidharth.v0/last_menstrual_period/subject"></mb-context>
</div>
