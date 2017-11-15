<script>
export default {
  name: 'schema-form',
  props: {
    collection: {
      type: Object,
      required: true,
      default: () => ({
        config: {},
        schema: {}
      })
    },
    confirm: {
      type: Function,
      required: true
    },
    cancel: {
      type: Function,
      required: true
    }
  },
  data () {
    return {
      formatterFields: {},
      formatterSchema: {},
      focusTarget: ''
    }
  },
  methods: {
    success () {
      this.confirm(this.formatterFields)
    },
    error () {
      this.cancel()
    },
    initializeFields () {
      /**
       * formatter item consistency
       */
    //   if (typeof this.collection.config !== 'object' || typeof this.collection.schema !== 'object') {
    //     return
    //   }
    //   const t = Object.assign({}, this.collection.config)
    //   if (Object.keys(this.collection.config).length === Object.keys(this.collection.schema.properties).length) {
    //     this.formatterFields = t
    //     return
    //   }
    //   for (const key in this.collection.schema.properties) {
    //     if (!t.hasOwnProperty(key)) {
    //       t[key] = ''
    //     }
    //   }
    //   this.formatterFields = t
    },
    initializeSchema () {
      /**
       * formatter 'required' property
       */
    //   if (typeof this.collection.schema !== 'object') {
    //     return
    //   }
    //   const t = Object.assign({}, this.collection.schema)
    //   for (const key in t.properties) {
    //     if (t.properties[key].required || t.required && t.required.indexOf(key) > -1) {
    //       t.properties[key].required = true
    //     } else {
    //       t.properties[key].required = false
    //     }
    //   }
    //   this.formatterSchema = t
    },
    changeFocusTarget (key) {
      this.focusTarget = key
    }
  },
  watch: {
    collection: function (value) {
      this.initializeFields()
      this.initializeSchema()
    }
  },
  mounted () {
    this.initializeFields()
    this.initializeSchema()
  }
}
</script>
<template lang="pug">
  el-form(:label-position="'right'", label-width="260px", class="customForm")
    el-form-item(v-for="(value, $key) of formatterSchema.properties",
      :label="$key",
      :key="$key",
      :required="value.required",
      :title="value.description || ''")
      el-switch(v-if="value.type === 'boolean'",
        v-model="formatterFields[$key]",
        on-value="true",
        off-value="false")
      el-input(v-else-if="value.type === 'number'", v-model.trim="formatterFields[$key]", @focus="(e) => {changeFocusTarget($key)}")
      el-input(v-else-if="!!value.final", v-model.trim="formatterFields[$key]", readonly)
      el-select(v-else-if="!!value.enum", v-model="formatterFields[$key]", @focus="(e) => {changeFocusTarget($key)}")
        el-option(v-for="(item, $index) of value.enum", :key="$index", :value="item", :label="item")
      el-input(v-else, v-model.trim="formatterFields[$key]", @focus="(e) => {changeFocusTarget($key)}")
      p(class="custom-help-msg", v-show="focusTarget === $key") {{value.description}}
    el-form-item(v-if="Object.keys(formatterFields).length > 0")
      el-button(type="primary", @click="success") {{this.$t('common.ok')}}
      el-button(@click="error") {{this.$t('common.cancel')}}
</template>

<style lang="scss">
.customForm {
  max-width: 800px;
  width: 100%;
  margin-top: 24px;
  margin-left: 0;
  .custom-help-msg {
    font-size: 13px;
    line-height: 1.1;
    color: #20a0ff;
    margin: 4px 0 0 0
  }
  .el-form-item {
    margin-bottom: 16px;
  }
}
</style>

