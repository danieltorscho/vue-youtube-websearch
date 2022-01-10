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

      <template v-if="keyword">
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
      </template>
      <template v-else>
        <div class="card-deck playlist">
          <div class="row">
            <div class="col-4 mb-5" v-for="(item, key) in playlist" :key="key">
                <div class="card">
                  <img :src="item.image" class="card-img-top">
                  <div class="card-body">
                    <h5 class="card-title">{{ item.title }}</h5>
                    <a :href="item.link" class="btn btn-primary playbtn btn-sm">Play</a>
                  </div>
                </div>
            </div>
          </div>
        </div>
      </template>

    </div>
  </div>
</template>

<script>
import _ from 'lodash'
import YouTubeSearch from 'youtube-api-search'

export default {
  data () {
    return {
      api: process.env.VUE_APP_YOUTUBE_API || 'PUT_YOUR_GOOGLE_YOUTUBE_API_HERE',
      playlist: [
        {
          title: 'Teklaaa',
          image: 'https://i.ytimg.com/vi/--kZEdjXcEI/hqdefault.jpg?sqp=-oaymwEZCPYBEIoBSFXyq4qpAwsIARUAAIhCGAFwAQ==&rs=AOn4CLARjHR0SrcC3XNb-8yEYcWm38n0rg',
          link: 'https://www.youtube.com/embed/?listType=playlist&list=PLc9Dd6RmU_QncpqobxcyRc1cfI-07Cq55'
        },
        {
          title: 'Oldies',
          image: 'https://i.ytimg.com/vi/Dl9iXBdJQtI/hqdefault.jpg?sqp=-oaymwEXCNACELwBSFryq4qpAwkIARUAAIhCGAE=&rs=AOn4CLC0IDDgjaVWumDMhQPUtozuIunvvQ',
          link: 'https://www.youtube.com/embed/?listType=playlist&list=PLc9Dd6RmU_QkDoAziDllzHh4rz5h5x2C9'
        },
        {
          title: 'Shanson',
          image: 'https://i.ytimg.com/vi/YXzS9DajdQ8/hqdefault.jpg?sqp=-oaymwEcCPYBEIoBSFXyq4qpAw4IARUAAIhCGAFwAcABBg==&rs=AOn4CLAudpv1CnVPiV2MtxdWP21m-51b1A',
          link: 'https://www.youtube.com/embed/?listType=playlist&list=PL1329C401520260DF'
        },
        {
          title: 'Russian',
          image: 'https://i.ytimg.com/vi/2-KcpAHFmg4/hqdefault.jpg?sqp=-oaymwEcCPYBEIoBSFXyq4qpAw4IARUAAIhCGAFwAcABBg==&rs=AOn4CLAQ_CmlolPTFaml9889ZahHOLWoPQ',
          link: 'https://www.youtube.com/embed/?listType=playlist&list=PL582CBFE5328AEB46'
        },
        {
          title: 'Mixed',
          image: 'https://i.ytimg.com/vi/3dCWaTw1Ejc/hqdefault.jpg?sqp=-oaymwEcCPYBEIoBSFXyq4qpAw4IARUAAIhCGAFwAcABBg==&rs=AOn4CLACWQkeiJPWXiXAz3kQ3j_t4UO9RQ',
          link: 'https://www.youtube.com/embed/?listType=playlist&list=PL952C3281C4C79F3B'
        },
        {
          title: 'Slovak',
          image: 'https://i.ytimg.com/vi/brT23KDweDk/hqdefault.jpg?sqp=-oaymwEZCNACELwBSFXyq4qpAwsIARUAAIhCGAFwAQ==&rs=AOn4CLB96MTATjNYdPL-juWEzRHhsxyucg',
          link: 'https://www.youtube.com/embed/?listType=playlist&list=PL51C850B5FE7B4DB4'
        }
      ],
      keyword: JSON.parse(localStorage.getItem('keyword')) || '',
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
      localStorage.setItem('keyword', JSON.stringify(this.keyword))
      YouTubeSearch({ key: this.api, term: this.keyword }, (videos) => {
        this.results = videos
      })
    }, 400)
  },

  mounted () {
    if (this.keyword) {
      this.onSearch()
    }
  }
}
</script>
