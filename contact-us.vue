/**
* @file contact-us.vue
* @description This Vue component renders a "Contact Us" form using Vuetify components.
* The form includes fields for first name, last name, email address, industry, service interest,
* and a radio group for requesting a video demo. The form validates input fields and displays
* alerts upon successful submission.
*
* @template
* - h-page-head: Displays the page title.
* - h-page-container: Container for the page content.
* - h-page-title: Displays the main title of the page.
* - h-page-description: Displays a description of the page.
* - v-container: Vuetify container for the form.
* - v-form: Vuetify form component with validation.
* - v-row, v-col: Vuetify grid components for layout.
* - v-text-field: Vuetify text field for input.
* - v-autocomplete: Vuetify autocomplete field for selecting industry and service interest.
* - v-radio-group, v-radio: Vuetify radio group for selecting video demo preference.
* - v-btn: Vuetify button for form submission.
*
* @script setup
* - Imports the `hAlerts` composable from Onify Helix.
* - Defines reactive `formData` object to store form input values.
* - Defines `isValid` ref to track form validity.
* - Defines `isSubmitting` ref to track form submission state.
* - Defines `vFormRef` ref to reference the form.
* - Defines `onSubmit` function to handle form submission, validate input, and display alerts.
*
* @methods
* - onSubmit: Validates the form, simulates form submission, displays a success alert, and resets the form.
*/

<template>
  <h-page-container page-title="Contact us">
    <v-container style="width: 600px" class="text-left mb-3">
      <h-page-title>Contact us</h-page-title>
      <h-page-description>Fill in the form to get in touch with us.</h-page-description>
      <v-form ref="vFormRef" v-model="isValid" validate-on="input" @submit.prevent="onSubmit">

        <!-- Name Fields -->
        <v-row>
          <v-col cols="12" md="6">
            <v-text-field v-model="formData.firstName" label="First Name" :rules="[hValidate.required]"
              hint="Enter your first name" prepend-icon="mdi:mdi-account" />
          </v-col>
          <v-col cols="12" md="6">
            <v-text-field v-model="formData.lastName" label="Last Name" :rules="[hValidate.required]"
              hint="Enter your last name" prepend-icon="mdi:mdi-account" />
          </v-col>
        </v-row>

        <!-- Email Field -->
        <v-text-field v-model="formData.email" label="Email Address" :rules="[hValidate.required, hValidate.email]"
          hint="Enter your email address" prepend-icon="mdi:mdi-email" />

        <!-- Industry Selection -->
        <v-autocomplete v-model="formData.industry"
          :items="['Technology', 'Finance', 'Healthcare', 'Education', 'Other']" label="Your Industry"
          :rules="[hValidate.required]" hint="Select your industry" prepend-icon="mdi:mdi-domain" clearable
          auto-select-first />

        <!-- Service Interest Selection -->
        <v-autocomplete v-model="formData.service" :items="['Consulting', 'Support', 'Implementation', 'Training']"
          label="What service are you interested in?" :rules="[hValidate.required]"
          hint="Select the service you're interested in" prepend-icon="mdi:mdi-briefcase" clearable auto-select-first />

        <!-- Video Demo Radio Group -->
        <v-radio-group v-model="formData.videoDemo" label="Would you like a video demo?" :rules="[hValidate.required]"
          prepend-icon="mdi:mdi-presentation-play" inline>
          <v-radio label="Yes" value="Yes" />
          <v-radio label="No" value="No" />
        </v-radio-group>

        <!-- Submit Button -->
        <v-btn class="mt-4" :disabled="!isValid || isSubmitting" color="primary" type="submit" :loading="isSubmitting">
          Submit
        </v-btn>

      </v-form>
    </v-container>
  </h-page-container>
</template>

<script setup>
const { hAlerts } = useHCommon();

const formData = reactive({});
const isValid = ref(false);
const isSubmitting = ref(false);
const vFormRef = ref(null);

const onSubmit = async () => {
  if (vFormRef.value && vFormRef.value.validate()) {
    isSubmitting.value = true;
    try {
      await new Promise(resolve => setTimeout(resolve, 1000)); // TODO: For testing purposes only
      //const response = await hHttpRequest({ url: 'my/workflows/run/contact-us', method: 'post', payload: formData }).response(); // TODO: Replace with actual workflow
      hAlerts.hAddAlert({
        type: 'info',
        title: `Thank you ${formData.firstName}!`,
        body: `We will contact you shortly via your email (${formData.email}).`,
        timeout: 5000
      });
      vFormRef.value.reset();
    } catch (err) {
      /* Do some error handling */
    } finally {
      isSubmitting.value = false;
    }
  }
};
</script>