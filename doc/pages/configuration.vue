<template>
  <v-container>
    <h1 class="display-1 mb-4">
      Configuration
    </h1>

    <h2 class="headline my-4">
      Options
    </h2>

    <p>Options can be passed to vjsf using the options prop or can be defined more locally on a specific property using the "x-options" annotation. Children properties will inherit from the options defined in their parents.</p>

    <v-simple-table dense dark class="mb-6">
      <thead>
        <tr>
          <th class="text-left">
            Key
          </th>
          <th class="text-left">
            Default
          </th>
          <th class="text-left">
            Description
          </th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="optionKey in Object.keys(defaultOptions)" :key="optionKey">
          <td>{{ optionKey }}</td>
          <td>{{ JSON.stringify(defaultOptions[optionKey], null, 2) }}</td>
          <td>{{ descriptions[optionKey] }}</td>
        </tr>
        <tr>
          <td>markdown</td>
          <td>window.markdownit().render if markdownit is available, identity function otherwise</td>
          <td>a function that takes a markdown text and returns HTML, used for descriptions and slots</td>
        </tr>
        <tr>
          <td>httpLib</td>
          <td>this.axios or this.$http or this.$axios or window.axios</td>
          <td>a simple HTTP client used to fetch select options from HTTP requests</td>
        </tr>
      </tbody>
    </v-simple-table>

    <h2 class="headline my-4">
      Messages
    </h2>
    <p>
      You can define a special "messages" option to overwrite the default localized messages.
    </p>
    <v-row>
      <v-col cols="2">
        <v-select v-model="locale" :items="Object.keys(localizedMessages)" label="Locale" hide-details dense />
      </v-col>
    </v-row>
    <v-simple-table dense dark class="mb-6">
      <thead>
        <tr>
          <th class="text-left">
            Message key
          </th>
          <th class="text-left">
            Default
          </th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="messageKey in Object.keys(localizedMessages[locale])" :key="messageKey">
          <td>{{ messageKey }}</td>
          <td>{{ localizedMessages[locale][messageKey] }}</td>
        </tr>
      </tbody>
    </v-simple-table>

    <h2 class="headline my-4">
      Icons
    </h2>
    <p>
      You can define a special "icons" option to overwrite the default icons. Default icons are initialized based on current iconfont configured on your Vuetify instance.
    </p>
    <v-row>
      <v-col cols="2">
        <v-select v-model="iconSet" :items="Object.keys(iconSets)" label="Icon set" hide-details dense />
      </v-col>
    </v-row>
    <v-simple-table dense dark class="mb-6">
      <thead>
        <tr>
          <th class="text-left">
            Icon key
          </th>
          <th class="text-left">
            Default
          </th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="iconKey in Object.keys(iconSets[iconSet])" :key="iconKey">
          <td>{{ iconKey }}</td>
          <td>{{ iconSets[iconSet][iconKey] }}</td>
        </tr>
      </tbody>
    </v-simple-table>

    <h2 class="headline my-4">
      Formatting functions
    </h2>
    <p>
      You can define a special "formats" option to overwrite the default formatting functions that are used to display some field values.
    </p>
    <v-simple-table dense dark class="mb-6">
      <thead>
        <tr>
          <th class="text-left">
            Message key
          </th>
          <th class="text-left">
            Default
          </th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="formatKey in Object.keys(formats)" :key="formatKey">
          <td>{{ formatKey }}</td>
          <td>{{ formats[formatKey] }}</td>
        </tr>
      </tbody>
    </v-simple-table>

    <h2 class="headline my-4">
      Annotations
    </h2>
    <p>
      Annotation is the name we give to special attributes defined on properties in your schema.
      These attributes are prefixed by "x-" to signify that they are not part of the standard JSON schema syntax.
      They should be ignored by validators and other tools.
    </p>
    <p />
    <v-simple-table dense dark class="mb-6">
      <thead>
        <tr>
          <th class="text-left" style="min-width:250px">
            Annotation key
          </th>
          <th class="text-left">
            Usage
          </th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <td>x-options</td>
          <td>Overwrite some of the options for this property and its children.</td>
        </tr>
        <tr>
          <td>x-display</td>
          <td>Used to switch the rendering mode of a property. We try to use as little of these as possible and rely on indications from the standard JSON schema syntax itself to render the form, but sometimes some ambiguity has to be resolved this way. For example this is used to force rendering sections as tabs or expansion panels.</td>
        </tr>
        <tr>
          <td>x-fromUrl / x-fromData</td>
          <td>Used to populate selects from the results of HTTP requests or some other part of the model.</td>
        </tr>
        <tr>
          <td>x-itemKey / x-itemTitle / x-itemIcon</td>
          <td>Used to configure rendering of options in selects.</td>
        </tr>
        <tr>
          <td>x-props</td>
          <td>An object of props to pass directly to the underlying Vuetify component.</td>
        </tr>
        <tr>
          <td>x-slots</td>
          <td>An object of textual contents to be displayed before and after the property or to be passed as slots to the underlying vuetify component.</td>
        </tr>
        <tr>
          <td>x-rules</td>
          <td>Some custom rule functions for validating this property.</td>
        </tr>
      </tbody>
    </v-simple-table>
  </v-container>
</template>

<script>
import { defaultOptions, formats, localizedMessages, iconSets } from '../../lib/utils/options'

export default {
  data() {
    return {
      defaultOptions,
      formats,
      localizedMessages,
      descriptions: {
        rootDisplay: 'equivalent of x-display annotation on the root object of the schema, can be "tabs" or "expansion-panels"',
        fieldProps: 'props passed to the underlying components for simple fields (v-text-field, v-select, etc.)',
        selectProps: 'props passed to the underlying components for select fields',
        tabsProps: 'props passed to the underlying v-tabs component when relevant',
        expansionPanelsProps: 'props passed to the underlying v-expansion-panels component when relevant',
        dialogProps: 'props passed to the underlying v-dialog component when relevant',
        colorPickerProps: 'props passed to the underlying v-color-picker component when relevant',
        timePickerProps: 'props passed to the underlying v-time-picker component when relevant',
        datePickerProps: 'props passed to the underlying v-date-picker component when relevant',
        removeAdditionalProperties: 'force the removal of properties found in the model but not defined in the schema',
        disableAll: 'force disabling all fields ignoring the readOnly attributes',
        hideReadOnly: 'by default read-only properties are rendered as disabled fields, set this to true to hide them entirely',
        deleteReadOnly: 'set this to true to not only hide read-only properties, but also remove them from the model',
        context: 'an optional contextual information object, properties from here can be used as variables in URL templates',
        rules: 'some custom rule functions that can be referenced by the x-rules annotation on properties',
        idPrefix: 'a prefix applied to generated ids if you want to prevent potential conflicts'
      },
      locale: 'en',
      iconSets,
      iconSet: 'mdi'
    }
  }
}
</script>

<style lang="css" scoped>
</style>
