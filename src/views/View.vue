<template>
  <div id="wrapper">
    <img :src="imageUrl" />
    <div id="imageDetails">
      <h3>Photographer Information</h3>
      <h3>Photographer: {{ imageDetails.photographer }}</h3>
      <h3>Photographer ID: {{ imageDetails.photographer_id }}</h3>
      <a
        :href="imageUrl"
        class="fa fa-download fa-2x"
        aria-hidden="true"
        download
      ></a>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      imageUrl: "",
      isLoading: false,
      imageDetails: {}
    };
  },
  created() {
    this.isLoading = true;
    fetch(
      `https://cors-anywhere.herokuapp.com/https://api.pexels.com/v1/photos/${this.$route.params.id}`,
      {
        method: "get",
        headers: {
          "Content-Type": "application/json",
          Authorization:
            "563492ad6f917000010000010f89364fd04c4d948ec5a7fa870ee83a"
        }
      }
    )
      .then(response => response.json())
      .then(result => {
        this.imageUrl = result.src.original;
        this.imageDetails = result;
      });
    this.isLoading = false;
  }
};
</script>

<style scoped lang="scss">
#wrapper {
  margin: 50px 100px;

  img {
    position: relative;
    width: 80%;
    height: 80vh;
    justify-content: center;
  }

  #imageDetails {
    :nth-child(1) {
      text-align: center;
      text-decoration: underline;
      color: chocolate;
    }
    :last-child {
      width: 25%;
      border-radius: 5px;
      background-color: #000;
      color: #fff;
      padding: 10px;

      &:hover {
        transform: scale(1.1);
        text-decoration: none;
      }
    }

    background: #fff;
    margin-top: 50px;
    position: relative;
    width: 80%;
    left: 10%;
    padding: 20px;
    box-sizing: border-box;
    box-shadow: 0 1px 1px rgba(0, 0, 0, 0.15), 0 2px 2px rgba(0, 0, 0, 0.15),
      0 4px 4px rgba(0, 0, 0, 0.15), 0 8px 8px rgba(0, 0, 0, 0.15),
      0 16px 16px rgba(0, 0, 0, 0.15), 0 32px 32px rgba(0, 0, 0, 0.15);
  }
}
</style>
