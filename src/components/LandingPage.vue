<template>
  <div>
    <div id="searchArea">
      <form id="searchForm" v-on:submit.prevent="searchForImages">
        <input
          type="search"
          v-model="searchValue"
          placeholder="Search for Images"
          name="searchBox"
          id="searchBox"
        />
        <button class="btn btn-success" type="submit">
          Send
        </button>
      </form>
    </div>
    <div v-show="!isLoading" id="searchResults">
      <div v-show="images.length < 1">
        <h3>
          No Search result
        </h3>
      </div>
      <div v-show="isLoading">
        LOADING!!!
      </div>
      <div id="searchResult" v-bind:key="image.id" v-for="image in images">
        <div id="image-part">
          <img v-bind:src="image.src.tiny" alt="picture 1" />
          <div id="router">
            <router-link :to="{ name: 'view', params: { id: image.id } }">
              View
            </router-link>
          </div>
        </div>
        <p>Photographer: {{ image.photographer }}</p>
      </div>
    </div>
    <div id="pagination" v-show="images.length > 1">
      <button type="submit" v-on:click="decreasePageNumber()" v-show="page > 1">
        Prev
      </button>
      <p>Page {{ page }} of {{ Math.ceil(totalCount / numberPerPage) }}</p>
      <button
        type="submit"
        v-on:click="increasePageNumber()"
        v-show="page < Math.ceil(totalCount / numberPerPage)"
      >
        Next
      </button>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      images: [],
      searchValue: "",
      isLoading: false,
      page: 1,
      totalCount: 0,
      numberPerPage: 20
    };
  },
  methods: {
    searchForImages: function() {
      this.isLoading = true;
      fetch(
        `https://cors-anywhere.herokuapp.com/https://api.pexels.com/v1/search?query=${this.searchValue}+query&per_page=${this.numberPerPage}&page=${this.page}`,
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
          this.images = result.photos;
          this.totalCount = result.total_results;
        });
      this.isLoading = false;
    },
    increasePageNumber: function() {
      this.page++;
      this.searchForImages();
    },
    decreasePageNumber: function() {
      this.page--;
      this.searchForImages();
    }
  },
  created() {}
};
</script>

<style scoped lang="scss">
#searchArea {
  background: #fff;
  padding: 20px;
  box-shadow: 0 1px 1px rgba(0, 0, 0, 0.15), 0 2px 2px rgba(0, 0, 0, 0.15),
    0 4px 4px rgba(0, 0, 0, 0.15), 0 8px 8px rgba(0, 0, 0, 0.15),
    0 16px 16px rgba(0, 0, 0, 0.15), 0 32px 32px rgba(0, 0, 0, 0.15);
}
#searchForm {
  display: grid;
  grid-template-columns: 3fr 1fr;
  grid-gap: 20px;

  @media only screen and (max-width: 960px) {
    grid-template-columns: 2fr 1fr;
  }

  input {
    padding: 10px;
  }
}
#searchResults {
  background: #fff;
  padding: 20px;
  box-shadow: 0 1px 1px rgba(0, 0, 0, 0.15), 0 2px 2px rgba(0, 0, 0, 0.15),
    0 4px 4px rgba(0, 0, 0, 0.15), 0 8px 8px rgba(0, 0, 0, 0.15),
    0 16px 16px rgba(0, 0, 0, 0.15), 0 32px 32px rgba(0, 0, 0, 0.15);
  display: grid;
  grid-template-columns: 1fr 1fr 1fr 1fr;
  grid-gap: 10px;
  overflow-x: auto;

  @media only screen and (max-width: 760px) {
    grid-template-columns: 1fr;
  }
}
#searchResult {
  transition: transform 1s;
  transition-timing-function: cubic-bezier(0.1, 0.7, 1, 0.1);
  &:hover {
    transform: scale(1.1);
    cursor: pointer;
  }

  #image-part {
    display: flex;
    flex-direction: column;
    position: relative;

    #router {
      position: absolute;
      width: 60%;
      left: 10%;
      bottom: 10%;
      border-radius: 5px;
      opacity: 0;
      background-color: #000;

      &:hover {
        opacity: 1;
      }

      @media only screen and (max-width: 960px) {
        opacity: 1;
      }

      a {
        color: #fff;
        &:hover {
          text-decoration: none;
          font-weight: bolder;
        }
      }
    }
  }

  img {
    height: 250px;
    width: 250px;
  }
}
#pagination {
  display: flex;
  justify-content: center;
  align-items: center;
  position: relative;
  margin-top: 25px;

  button {
    border: 1px solid;
    border-radius: 5px;
    margin: 0px 25px;
  }
}
</style>
