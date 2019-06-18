<script> /*eslint-disable */</script>

<template>
     <div id="upload">
        <label>File
            <input type="file" id="file" ref="file" v-on:change="handleFileUpload()" />
        </label>
        
        <button v-on:click="submitFile()">Submit</button>
</div>

</template>
<script>
export default {
    data () {
    return {
      file: '',
      name: ''
    }
  },
  methods: {
    submitFile() {
      let formData = new FormData();
      formData.append('file', this.file);
      console.log('>> formData >> ', formData);

      // You should have a server side REST API 
      axios.post('/api/user/1/uplaod',
          formData, {
            headers: {
              'Content-Type': 'multipart/form-data',
              'auth': localStorage.getItem('auth') // token
            }
          }
        ).then(function () {
          console.log('SUCCESS!!');
        })
        .catch(function () {
          console.log('FAILURE!!');
        });
    },
    handleFileUpload() {
      this.file = this.$refs.file.files[0];
      console.log('>>>> 1st element in files array >>>> ', this.file);
    }
  }
}
</script>
