<template>
  <div class="index-left-component">
    <div class="index-left-content">
      <div class="speak-show">
        <div class="lookup">
          <div>{{locale.topic}}</div>
          <a href="/create/topic" target="_blank">
            <book-icon :size="'14'"/>
            New
          </a>
        </div>
        <div class="search" :class="{'is-active':isActive}">
          <input v-model="searchContent" @focus="focusIn" @focusout="focusOut" type="text" :placeholder="locale.indexPageLeftSearchPlaceholder">
        </div>
        <div class="show">
          <ul>
            <li v-for="(item, key) in topicsData" :key="key">
              <a :href="`/topic/public/${item.topicId}`">
                <div>
                  <book-open-icon :size="'16'"/>
                </div>
                <p>{{item.topicTitle}}</p>
              </a>
            </li>
          </ul>
        </div>
      </div>
      <div class="apply-certification">
        <div class="title">
          {{locale.indexPageLeftTitle}}
        </div>
        <div class="desc">
          {{locale.indexPageLeftDesc}}
        </div>
        <a @click="()=>this.$message('暂未上线')">{{locale.certification}}</a>
      </div>
    </div>
  </div>
</template>

<script>
import {BookIcon, BookOpenIcon,} from "vue-feather-icons"
import {HttpPost} from "@/http/indexPage";

export default {
  name: "IndexLeftComponent",
  components: {
    BookIcon, BookOpenIcon,
  },
  data() {
    return {
      locale: this.$locale,
      isActive: false,
      isActiveBak: false,
      searchContent: "",
      topicsData:""
    }
  },
  methods: {
    focusIn() {
      this.isActive = true
      this.isActiveBak = true
    },
    focusOut() {
      this.isActive = false
      this.searchTopic()
    },

    dataInit() {
      HttpPost("/api/post/select/me/topics").then(ret=>{
        let lst = ret.data.code.split(" ")
        if (lst[0] !== "200") {
          this.$message.error(lst[1])
          return
        }
        this.topicsData = ret.data.topicAbsList
      }).catch(e=>{
        console.log(e)
      })
    },

    searchTopic() {
      if (!this.isActiveBak || this.searchContent === "") return
      HttpPost("/api/post/select/search/me/topics",{"name":"abc"}).then(ret=>{
        console.log(ret)

        let lst = ret.data.code.split(" ")
        if (lst[0] !== "200") {
          alert(lst[1])
          return
        }
        this.topicsData = ret.data.topicAbsList
      }).catch(e=>{
        console.log(e)
      })
    }
  },
  mounted() {
    this.dataInit()
  }
}
</script>

<style lang="scss" scoped>
@import "~@/api/GlobalApi.scss";
.index-left-component {
  width: 100%;

  .index-left-content {
    width: 100%;
    display: flex;
    flex-direction: column;
    justify-content: flex-start;
    align-items: flex-start;

    .speak-show {
      margin: 32px 0 16px 0;
      width: inherit;

      .lookup {
        width: 100%;
        display: flex;
        justify-content: space-between;
        align-items: center;
        height: 28px;
        margin-bottom: 4px;
        font-size: 14px;

        div {
          font-weight: 800;
        }

        a {
          border-radius: 6px;
          background: $index-page-main-background-color-green;
          color: white;
          height: 100%;
          display: flex;
          justify-content: center;
          align-items: center;
          width: 67px;
          transition: all .2s;
        }
        a:hover {
          background: $index-page-main-background-color-green-2;
        }

        @media screen and (max-width: $min) {
          a {
            display: none;
          }
        }
      }

      .search {
        width: inherit;
        margin-bottom: 16px;
        padding: 5px 12px;
        border-radius: 6px;
        border: 1px solid $index-page-main-border-color-grey;
        transition: all .2s;

        &.is-active {
          border: 1px solid $index-page-main-middle-font-color-blue;
          box-shadow: 0 0 6px $index-page-main-middle-font-color-blue-2;
        }

        input {
          border: none;
          width: 100%;
          background: none;
        }
      }

      .show {

        ul {
          display: flex;
          flex-direction: column;
          justify-content: flex-start;
          align-items: flex-start;
          width: 100%;

          li {
            width: inherit;
            a {
              width: 100%;
              height: 29px;
              display: flex;
              justify-content: flex-start;
              align-items: center;

              div {
                margin-right: 8px;
                color: $index-page-main-font-color-grey-3;
              }
              div.private {
                color: $index-page-main-font-color-yellow;
              }
              p {
                flex: 10;
                text-align: start;
                text-overflow: ellipsis;
                white-space: nowrap;
                overflow: hidden;
              }
            }
          }
        }
      }

    }

    @media screen and (max-width: 767px) {
      .speak-show {
        padding: 4px 8px;
        border-radius: 6px;
        border: 1px solid $index-page-main-middle-border-color;
        background: $index-page-main-middle-card-background;

        .search {
          background: $index-page-main-middle-background-grey;
        }
      }
    }

    .apply-certification {
      margin: 16px 0;
      display: flex;
      flex-direction: column;
      justify-content: flex-start;
      align-items: flex-start;

      .title {
        width: 100%;
        padding-top: 16px;
        margin-bottom: 4px;
        color: black;
        font-weight: 600;
        font-size: 14px;
        border-top: 1px solid $index-page-main-border-color-grey;
        text-align: start;
      }
      .desc {
        margin-bottom: 10px;
        font-size: 12px;
        text-align: start;
      }
      a {
        padding: 3px 12px;
        margin-right: 8px;
        border-radius: 6px;
        border: 1px solid $index-page-main-border-color-grey-2;
        font-size: 12px;
        min-width: 120px;
        height: 28px;
        background: $index-page-main-background-color-grey;
        color: $index-page-main-middle-font-color-blue;
        transition: all .2s;
      }
      a:hover {
        color: white;
        background: $index-page-main-middle-font-color-blue;
      }
    }

    @media screen and (max-width: 767px) {
      .apply-certification {
        display: none;
      }
    }

  }


}

</style>