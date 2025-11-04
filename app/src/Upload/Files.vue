<template lang="pug">
  div.upload-files
    .panel.panel-default(:class="{'panel-primary': !disabled}")
      .panel-heading
        span.pull-right(v-show="bucketSize > 0") {{ humanFileSize(bucketSize) }}
        strong {{ $root.lang.files }}
      .panel-body
        .empty-files-big-plus(
          :style="{cursor: disabled ? 'default' : 'pointer'}",
          v-show="files.length === 0",
          tabindex="0",
          role="button",
          @click="triggerFileInput()",
          @keydown.enter.prevent="triggerFileInput()",
          @keydown.space.prevent="triggerFileInput()"
        )
          a
            icon(name="plus", scale="4")
            br
            |  {{ $root.lang.dropFilesHere }}
        table.table.table-striped
          tbody
            tr(v-for="file in files")
              td.file-icon
                file-icon(:file="file._File")
              td
                p
                  strong  {{ file.name }}
                  small  ({{ file.humanSize }})
                p
                  input.form-control.input-sm(type="text", :placeholder="$root.lang.comment", v-model="file.comment", :disabled="disabled")
                .alert.alert-danger(v-if="file.error")
                  icon.fa-fw(name="exclamation-triangle")
                  |  {{ file.error }}
                .progress(v-show="!file.error && (state === 'uploading' || state === 'uploaded')")
                  .progress-bar.progress-bar-success.progress-bar-striped(:style="{width: file.progress.percentage+'%'}", :class="{active:!file.uploaded}")
              td.btns
                a(
                  style="cursor:pointer",
                  @click="$store.dispatch('upload/removeFile', file)",
                  @keydown.enter.prevent="$store.dispatch('upload/removeFile', file)",
                  @keydown.space.prevent="$store.dispatch('upload/removeFile', file)",
                  v-show="!disabled || bucketSizeError",
                  tabindex="0",
                  role="button"
                )
                  icon(name="times")

        input#fileInput(type="file", @change="$store.dispatch('upload/addFiles', $event.target.files)", multiple="", :disabled="disabled", style="display: none")
        .text-right
          a.btn.btn-success.btn-sm(
            @click="triggerFileInput()",
            @keydown.enter.prevent="triggerFileInput()",
            @keydown.space.prevent="triggerFileInput()",
            :disabled="disabled",
            v-show="files.length>0",
            tabindex="0",
            role="button"
          )
            icon(name="plus-circle")
</template>


<script type="text/babel">
  import dragDrop from 'drag-drop';
  import 'vue-awesome/icons/exclamation-triangle'
  import 'vue-awesome/icons/plus'
  import 'vue-awesome/icons/plus-circle'
  import 'vue-awesome/icons/times'
  import { mapGetters, mapState } from 'vuex';
  import FileIcon from '../common/FileIcon.vue';
  import { humanFileSize } from "./store/upload";

  export default {
    name: 'Files',

    components: { FileIcon },

    computed: {
      ...mapState('upload', ['files']),
      ...mapState(['state',]),
      ...mapGetters('upload', ['bucketSize', 'bucketSizeError']),
      ...mapGetters(['disabled']),
    },

    mounted() {
      // init drop files support on <body>
      this.dragDropCleanup = dragDrop('body', files => this.$store.dispatch('upload/addFiles', files));
    },

    watch: {
      state: function(state) {
        if(state === 'uploading') {
          this.dragDropCleanup();
        }
      }
    },

    methods: {
      humanFileSize,
      triggerFileInput() {
        if (this.disabled) return;
        const input = document.getElementById('fileInput');
        if (input) input.click();
      },
    }
  };
</script>

<style scoped>
  .upload-files {
    width: 100%;
  }

  .empty-files-big-plus {
    text-align: center;
    padding: 60px 20px;
    transition: all 0.3s ease;
  }

  .empty-files-big-plus:hover {
    transform: scale(1.02);
  }

  .empty-files-big-plus icon {
    color: #6366f1;
    margin-bottom: 16px;
    display: block;
  }

  .empty-files-big-plus a {
    color: #6366f1;
    font-weight: 500;
    text-decoration: none;
    font-size: 18px;
    font-family: 'Inter', sans-serif;
  }

  .empty-files-big-plus a:hover {
    color: #4f46e5;
    text-decoration: underline;
  }

  .table {
    margin-top: 16px;
  }

  .table tr {
    transition: all 0.2s ease;
  }

  .table tr:hover {
    background: #f9fafb !important;
  }

  td.file-icon {
    width: 60px;
    text-align: center;
    vertical-align: middle;
  }

  td.btns {
    width: 40px;
    text-align: center;
    vertical-align: middle;
  }

  td.btns a {
    color: #ef4444;
    cursor: pointer;
    transition: all 0.2s ease;
    display: inline-block;
    padding: 4px;
  }

  td.btns a:hover {
    color: #dc2626;
    transform: scale(1.2);
  }

  .progress {
    margin-top: 8px;
    height: 8px;
  }

  .alert-danger {
    margin-top: 8px;
    padding: 12px;
  }

  .form-control.input-sm {
    margin-top: 8px;
    font-size: 14px;
  }

  .text-right {
    margin-top: 16px;
  }

  .text-right .btn {
    margin-top: 8px;
  }

  p {
    margin-bottom: 8px;
  }

  p strong {
    font-family: 'Inter', sans-serif;
    font-weight: 600;
    color: #1f2937;
  }

  p small {
    color: #6b7280;
    font-weight: 400;
  }
</style>