<template>
  <div class="uploader">
    <b-card class="mb-3" header="Upload an Image of a Currency Note" header-tag="header">
       <b-form @submit.prevent="uploadImage">
            <b-form-file v-model="image" accept="image/*"></b-form-file>
            <b-button type="submit" variant="primary" class="mt-3">Upload</b-button>
          </b-form>
          <div v-if="prediction" class="mt-4">
            <h4>Prediction</h4>
            <p>{{ prediction }}</p>
            <b-img :src="uploadedImageUrl" fluid alt="Uploaded Image"></b-img>
          </div>
    </b-card>
  </div>
</template>

<script>
export default {
  data() {
    return {
      image: null,
      prediction: null,
      uploadedImageUrl: null,
    };
  },
  methods: {
    async uploadImage() {
      if (!this.image) {
        alert('Please select an image to upload.');
        return;
      }

      const formData = new FormData();
      formData.append('file', this.image);

      try {
        const response = await this.$axios.post('YOUR_API_ENDPOINT', formData);
        this.prediction = response.data.prediction;
        this.uploadedImageUrl = URL.createObjectURL(this.image);
      } catch (error) {
        console.error('Error uploading image:', error);
        alert('Error uploading image. Please try again.');
      }
    },
  },
};
</script>