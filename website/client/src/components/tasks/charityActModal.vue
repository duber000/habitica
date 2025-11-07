<template>
  <b-modal
    id="charity-act-modal"
    :title="$t('logCharityAct')"
    size="md"
    @ok="submitCharityAct"
    @hidden="resetForm"
  >
    <form @submit.stop.prevent="submitCharityAct">
      <div class="form-group">
        <label class="mb-1">
          {{ $t('description') }} <span class="text-danger">*</span>
        </label>
        <textarea
          v-model="charityAct.description"
          class="form-control"
          :placeholder="$t('charityDescriptionPlaceholder')"
          rows="3"
          required
        ></textarea>
      </div>

      <div class="form-group">
        <label class="mb-1">
          {{ $t('category') }} <span class="text-danger">*</span>
        </label>
        <select
          v-model="charityAct.category"
          class="form-control"
          required
        >
          <option value="">{{ $t('selectCategory') }}</option>
          <option value="volunteer">{{ $t('categoryVolunteer') }}</option>
          <option value="help">{{ $t('categoryHelp') }}</option>
          <option value="teach">{{ $t('categoryTeach') }}</option>
          <option value="support">{{ $t('categorySupport') }}</option>
          <option value="donate">{{ $t('categoryDonate') }}</option>
          <option value="other">{{ $t('categoryOther') }}</option>
        </select>
      </div>

      <div class="form-group">
        <label class="mb-1">
          {{ $t('duration') }} ({{ $t('minutes') }})
        </label>
        <input
          v-model.number="charityAct.duration"
          class="form-control"
          type="number"
          min="1"
          :placeholder="$t('optionalDuration')"
        >
      </div>

      <div class="form-group">
        <label class="mb-1">
          {{ $t('additionalNotes') }}
        </label>
        <textarea
          v-model="charityAct.notes"
          class="form-control"
          :placeholder="$t('optionalNotes')"
          rows="2"
        ></textarea>
      </div>
    </form>

    <template #modal-footer="{ ok, cancel }">
      <button
        class="btn btn-secondary"
        @click="cancel()"
      >
        {{ $t('cancel') }}
      </button>
      <button
        class="btn btn-primary"
        :disabled="!isFormValid"
        @click="ok()"
      >
        {{ $t('logAct') }}
      </button>
    </template>
  </b-modal>
</template>

<script>
export default {
  name: 'CharityActModal',
  data () {
    return {
      charityAct: {
        description: '',
        category: '',
        duration: null,
        notes: '',
      },
    };
  },
  computed: {
    isFormValid () {
      return this.charityAct.description.trim() !== '' && this.charityAct.category !== '';
    },
  },
  methods: {
    submitCharityAct (bvModalEvent) {
      // Prevent modal from closing if form is invalid
      if (!this.isFormValid) {
        bvModalEvent.preventDefault();
        return;
      }

      // Emit the charity act data to parent component
      this.$emit('charity-act-submitted', {
        description: this.charityAct.description.trim(),
        category: this.charityAct.category,
        duration: this.charityAct.duration || undefined,
        notes: this.charityAct.notes.trim() || undefined,
      });

      // Reset form
      this.resetForm();
    },
    resetForm () {
      this.charityAct = {
        description: '',
        category: '',
        duration: null,
        notes: '',
      };
    },
  },
};
</script>

<style scoped>
.text-danger {
  color: #dc3545;
}
</style>
