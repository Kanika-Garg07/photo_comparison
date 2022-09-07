<template>
  <q-page class="flex flex-center">
    <q-card class="main_card q-mt-md q-mb-md">
      <q-card-section v-if="imageListData.length>0">
        <div class="row">
          <div class="col-12 q-pa-md">
            <div class="card-data">
              <div
                class="card"
                v-for="image in imageListData"
                :key="image.id"
              >
                <div class="card-img">
                  <img
                    :src="image.url"
                    alt="Image Thumbnail"
                  />
                </div>
                <div>
                  <div class="image-title">
                    <span :title="image.title">
                      {{ image.title }}
                    </span>
                  </div>
                  <div class="image_id">
                    <span :title="image.id">
                      {{ image.id }}
                    </span>
                  </div>
                  <div class="image-url">
                    <a :href="image.url">
                      {{ image.url }}
                    </a>
                  </div>
                  <q-btn class="add-btn"
                    :label="ifCardSelected(image) ? 'Compare' : 'Remove'" 
                    :color="ifCardSelected(image) ? 'primary' : ''" 
                    @click="addToComparisonTable(image)" />
                </div>
              </div>
            </div>
          </div>
        </div>
      </q-card-section>
      <q-card-section>
        <q-table
          v-if="tableData.length>0"
          title="Comparison Table"
          :columns="columns"
          :data="tableData"
          separator="cell"
          row-key="name"
        />
      </q-card-section>
    </q-card>
  </q-page>
</template>

<script>
import axios from 'axios';

export default {

  data () {
    return {
      columns:[
        { name: 'Name', label: 'Name', align: 'left', field: 'name' },
        { name: 'ID', label: 'ID', align: 'left', field: 'ID' },
        { name: 'URL', label: 'URL', align: 'left', field: 'URL' },
        { name: 'Title', label: 'Title', align: 'left', field: 'Title' }
      ],
      tableData: [],
      imageListData: [],
      selectedCards: []
    }
  },

  methods: {
    getImageData () {
      axios.get('https://jsonplaceholder.typicode.com/photos').then((res)=>{
        for (let i = 0; i < 10; i++) {
          this.imageListData.push(res.data[i])
        }
      })
    },

    addToComparisonTable (image) {
      if (this.selectedCards.includes(image.id)) {
        const index = this.selectedCards.indexOf(image.id)
        if (index > -1) {
          this.selectedCards.splice(index, 1)
        }
      } else {
        this.selectedCards.push(image.id)
      }
      this.tableData = []
      for (let i = 0; i < this.imageListData.length; i++) {
        const image = this.imageListData[i]
        if (this.selectedCards.includes(image.id)) {
          this.tableData.push({ name: 'Photo ' + (i + 1), ID: image.id, URL: image.url, Title: image.title })
        }
      }
    },
    
    ifCardSelected (image) {
      return !this.selectedCards.includes(image.id);
    },
  },

  mounted(){
    this.getImageData();
  }
}
</script>
<style>

.image-title {
  text-transform: capitalize;
  height: 80px;
  overflow: hidden;
  text-overflow: ellipsis;
  display: -webkit-box;
  -webkit-line-clamp: 3;
  -webkit-box-orient: vertical;
  color: black;
  text-align: center;
  display: flex;
  flex-wrap: wrap;
}

.main_card {
  width: 100%;
  min-height: calc(100vh - 80px);
  max-width: 1200px;
}

.add-btn {
  margin-top:10px;
  width: 100%;
}

.image_id {
  text-align: center;
  font-weight: bold;
  color: #000;
  font-size: 20px;
}

.image-url {
  font-size: 9px;
  text-transform: lowercase;
  text-align: center;
}

@import url("https://fonts.googleapis.com/css2?family=DM+Mono:wght@300;400;500&display=swap");

.card-data {
  display: flex;
  flex-wrap: wrap;
  margin-top: -10px;
  margin-left: -10px;
}

.card {
  display: flex;
  flex-direction: column;
  padding: 1.5rem;
  width: calc(25% - 10px);
  margin-left: 10px;
  margin-top: 10px;
  position: relative;
}

.card {
  border-radius: 16px;
  background: #909090;
  /* box-shadow: -1rem 0 3rem #000; */
  background-repeat: no-repeat;
  background-position: center;
  background-size: contain;
  transition: 0.2s;
}

.card-img {
  display: flex;
  flex-wrap: wrap;
  background: #909090;
  background-repeat: no-repeat;
  background-position: center;
  background-size: contain;
  /* margin-top: -24px; */
  transition: 0.2s;
}

.card-img img {
  width: 100%;
  border-radius: 16px;
  margin-bottom: 10px;
  /* margin: 0 -24px; */
}

.tags {
  margin: 1rem 0 2rem;
  padding: 0.5rem 0 1rem;
  line-height: 2;
  margin-bottom: 0;
}

.tags a {
  font-style: normal;
  font-weight: 700;
  font-size: 0.5rem;
  color: #7a7a8c;
  text-transform: uppercase;
  font-size: 0.66rem;
  border: 3px solid #28242f;
  border-radius: 2rem;
  padding: 0.2rem 0.85rem 0.25rem;
  position: relative;
}


</style>
