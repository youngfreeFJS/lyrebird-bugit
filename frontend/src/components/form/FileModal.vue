<template>
  <Modal v-model="isDisplayFile" width="60" :styles="{top: '30px'}">
    <div slot="header" style="text-align:center">
      <p>
        <Icon type="md-document"/>
        <span style="padding-left:10px;">{{attachmentName}}</span>
      </p>
    </div>
    <div>
      <p v-if="isImageFile">
        <ImageEditor
          :attachmentId="attachmentId"
          :src="'/plugins/bugit/api/attachments/' + this.attachmentId"
        />
      </p>
      <p v-else>
        <FileViewer :attachmentId="attachmentId"/>
      </p>
    </div>
    <div slot="footer">
      <p v-if="isImageFile">
        <Button long type="success" @click="saveImage()">Save</Button>
      </p>
      <p v-else>
        <Button long style="background-color:#dcdee2" @click="isDisplayFile = false">Close</Button>
      </p>
    </div>
  </Modal>
</template>

<script>
import ImageEditor from '@/components/form/ImageEditor.vue'
import FileViewer from '@/components/form/FileViewer.vue'

export default {
  components: {
    ImageEditor,
    FileViewer
  },
  created() {
    this.$bus.$on('displayFile', this.displayFile)
  },
  data() {
    return {
      isDisplayFile: false,
      attachmentName: null,
      attachmentId: null
    }
  },
  computed: {
    isImageFile() {
      if (!this.attachmentName) {
        return false
      }
      const imageFileSuffix = ['png', 'jpg', 'jpeg', 'webp']

      let nameSplit = this.attachmentName.split('.')
      let nameSuffix = nameSplit[nameSplit.length - 1]

      return imageFileSuffix.indexOf(nameSuffix) > -1
    }
  },
  methods: {
    displayFile(data) {
      this.isDisplayFile = true
      this.attachmentName = data.name
      this.attachmentId = data.id
    },
    saveImage() {
      this.$bus.$emit('saveImage')
      this.isDisplayFile = false
    }
  }
}

</script>
