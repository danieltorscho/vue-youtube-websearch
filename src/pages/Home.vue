<template>
  <div class="home">
    <div class="container">

      <div class="row">
        <div class="col">
          <div class="input-group mb-3">
            <input
              type="text"
              class="form-control"
              placeholder="Search for a video..."
              v-model="keyword"
              @input="onSearch"
            >
            <div class="input-group-append">
              <button class="btn btn-primary" type="button" id="button-addon2">Search</button>
            </div>
          </div>
        </div>
      </div>

      <div class="card mb-2" v-for="video in results" :key="video.etag">
        <div class="row">
          <div class="col-md-4">
            <img :src="video.snippet.thumbnails.medium.url" class="card-img" :height="video.snippet.thumbnails.medium.height">
            <a :href="`https://www.youtube.com/embed/${video.id.videoId}`" class="btn btn-primary playbtn">Play</a>
          </div>
          <div class="col-md-8">
            <div class="card-body">
              <h5 class="card-title">{{ video.snippet.title }}</h5>
              <p class="card-text description">{{ video.snippet.description }}</p>
              <p class="card-text">
                <small class="text-muted">{{ video.snippet.channelTitle }}</small>
                <small class="text-muted">{{ video.snippet.publishedAt | date }}</small>
                </p>
            </div>
          </div>
        </div>
      </div>

    </div>
  </div>
</template>

<script>
import _ from 'lodash'
import YouTubeSearch from 'youtube-api-search'

export default {
  data () {
    return {
      api: 'AIzaSyBHvNQW55VkxjNaP0tECyCpnkRBcEiNhIc',
      keyword: '',
      results: []
    }
  },

  filters: {
    date (value) {
      return new Date(value).toLocaleString()
    }
  },

  methods: {
    onSearch: _.debounce(function () {
      YouTubeSearch({ key: this.api, term: this.keyword }, (videos) => {
        this.results = videos
      })
    }, 400)
  }
}
</script>
