<template>
  <div class="container mt-5">
    <form>
      <div v-for="field in fields" :key="field.model" class="mb-3">
        <label :for="field.model" class="form-label">{{ field.label }}:</label>

        <template v-if="!field.type || field.type === 'select'">
          <select
            v-model="formData[field.model]"
            @change="checkCondition(field)"
            class="form-select"
          >
            <option v-for="option in field.options" :key="option.value" :value="option.value">
              {{ option.name }}
            </option>
          </select>
        </template>

        <template v-if="field.type === 'number'">
          <input
            :type="field.type"
            v-model="formData[field.model]"
            @change="checkCondition(field)"
            :id="field.model"
            class="form-control"
          />
        </template>

        <div v-if="showExtraInput[field.model]" class="mt-3">
          <template v-if="field.extraInputs">
            <div v-for="extraInput in field.extraInputs" :key="extraInput.model" class="mb-3">
              <label :for="extraInput.model" class="form-label">{{ extraInput.label }}:</label>
              <input
                type="text"
                v-model="formData[extraInput.model]"
                :id="extraInput.model"
                class="form-control"
              />
            </div>
          </template>

          <template v-else>
            <label :for="field.extraInputModel" class="form-label"
              >{{ field.extraInputLabel }}:</label
            >
            <input
              type="text"
              v-model="formData[field.extraInputModel]"
              :id="field.extraInputModel"
              class="form-control"
            />
          </template>
        </div>
      </div>
    </form>
  </div>
</template>

<script>
import { ref, reactive, onMounted } from 'vue';
import formRules from './formRules.json';

export default {
  setup() {
    const formData = reactive({});
    const showExtraInput = reactive({});
    const fields = ref(formRules.fields);

    const initializeForm = () => {
      fields.value.forEach((field) => {
        formData[field.model] = '';
        if (field.extraInputs) {
          field.extraInputs.forEach((extraInput) => {
            formData[extraInput.model] = '';
          });
        } else {
          formData[field.extraInputModel] = '';
        }
        showExtraInput[field.model] = false;
      });
    };

    const checkCondition = (field) => {
      const value = formData[field.model];
      const option = field.options ? field.options.find((opt) => opt.value === value) : null;
      const condition = field.condition;

      if (option && option.requiresInput) {
        showExtraInput[field.model] = true;
      } else if (condition && condition.lessThan !== undefined) {
        showExtraInput[field.model] = value < condition.lessThan;
      } else {
        showExtraInput[field.model] = false;
      }

      if (!showExtraInput[field.model]) {
        if (field.extraInputs) {
          field.extraInputs.forEach((extraInput) => {
            formData[extraInput.model] = '';
          });
        } else {
          formData[field.extraInputModel] = '';
        }
      }
    };

    onMounted(() => {
      initializeForm();
    });

    return {
      formData,
      showExtraInput,
      fields,
      checkCondition,
    };
  },
};
</script>
