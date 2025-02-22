<template>
  <div class="bg-light" style="display: flex; flex-direction: column">
    <div v-if="!isHeaderExpanded" class="header-gradient"></div>
    <div
      :class="{ 'detail-block': !isHeaderExpanded }"
      class="container-fluid extended p-2 pb-0 p-sm-5"
    >
      <div class="row">
        <div
          class="col-md-8 d-flex justify-content-between flex-column flex-sm-row w-100"
        >
          <div style="margin-right: 20px">
            <nuxt-link
              :to="returnLink"
              class="btn-circle arrow-left filled-dark return-button"
            >
              <img src="/icons/ic_arrow.svg" class="return-icon" />
            </nuxt-link>
          </div>

          <div>
            <h1>#{{ component.title }}</h1>

            <div class="d-flex">
              <div class="full-width mr-2">
                <div>
                  {{ component.descriptionAbstract }}
                </div>
                <div class="text-muted mt-4">
                  {{ component.description }}
                </div>
              </div>
            </div>
          </div>
        </div>
        <div class="col-md-4 detail-border pl-md-5">
          <div class="d-table w-100 mr-2 ml-2 m-sm-0">
            <div class="d-table-row">
              <div class="d-table-cell pr-2">Author:</div>
              <div class="d-table-cell">
                <div v-for="author in component.authors" :key="author.name">
                  {{ author.name }}
                </div>
              </div>
            </div>
            <div
              v-if="
                component.copyrightHolders &&
                  component.copyrightHolders.length > 0
              "
              class="d-table-row"
            >
              <div class="d-table-cell pr-2">Copyright holder:</div>
              <div class="d-table-cell">
                <div
                  v-for="author in component.copyrightHolders"
                  :key="author.name"
                >
                  {{ author.name }}
                </div>
              </div>
            </div>
            <div class="d-table-row">
              <div class="d-table-cell pr-2">Category:</div>
              <div class="d-table-cell">
                <div
                  v-for="tag in component.searchTags"
                  :key="tag"
                  class="text-capitalize"
                >
                  {{ tag }}
                </div>
              </div>
            </div>
            <div class="d-table-row">
              <div class="d-table-cell pr-2">License:</div>
              <div class="d-table-cell">
                <a
                  v-if="component.license && component.license.seeAlso"
                  :href="component.license.seeAlso[0]"
                  :title="component.license.name"
                  target="_blank"
                  >{{ component.license.licenseId }}</a
                >
                <div v-else-if="component.license">
                  {{ component.license.licenseId }}
                </div>
                <div v-else>{{ component.licenseString }}</div>
              </div>
            </div>
            <div class="d-table-row">
              <div class="d-table-cell pr-2">
                First Published:
              </div>
              <div class="d-table-cell">
                {{ $d(new Date(component.datePublished)) }}
              </div>
            </div>
            <div class="d-table-row">
              <div class="d-table-cell pr-2">Current Version:</div>
              <div class="d-table-cell">
                {{ component.versions[0].versionTag }}
              </div>
            </div>
            <div class="d-table-row">
              <div class="d-table-cell pr-2 text-nowrap">Last Update:</div>
              <div class="d-table-cell">
                {{ $d(new Date(component.dateUpdated)) }}
              </div>
            </div>
            <div v-if="component.repositoryUrl" class="d-table-row">
              <div class="d-table-cell pr-2 text-nowrap">Repository:</div>
              <div class="d-table-cell">
                <a :href="component.repositoryUrl" target="_blank">open</a>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
    <div class="d-flex justify-content-center flex-row">
      <span
        v-if="!isHeaderExpanded"
        @click="toggleHeaderHeight"
        class="chevron bottom mr-2 expanding-button"
      ></span>
      <span
        v-else
        @click="toggleHeaderHeight"
        class="chevron top mr-2 expanding-button"
      ></span>
    </div>
    <div class="container-fluid extended pl-sm-5">
      <div
        class="row col-md-8 d-flex justify-content-start flex-column flex-md-row w-100"
      >
        <div style="width: 90px"></div>
        <div class="d-flex flex-row tab-buttons">
          <div
            @click="$emit('set-show-preview', true)"
            :class="[showPreview ? 'tab-button' : 'tab-button-disabled']"
            class="text-uppercase"
          >
            preview
          </div>
          <div
            @click="$emit('set-show-preview', false)"
            :class="[showPreview ? 'tab-button-disabled' : 'tab-button']"
            class="text-uppercase"
          >
            readme
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    component: {
      type: Object,
      default: () => {
        return {}
      }
    },
    returnLink: {
      type: String,
      default: ''
    },
    showPreview: {
      type: Boolean,
      default: true
    }
  },
  data() {
    return {
      isHeaderExpanded: false
    }
  },
  methods: {
    toggleHeaderHeight() {
      this.isHeaderExpanded = !this.isHeaderExpanded
    }
  }
}
</script>
<style lang="scss">
@media (max-width: 576px) {
  .return-button {
    height: 35px;
    width: 35px;
  }

  .return-icon {
    height: 15px;
    width: 15px;
    transform: scale(0.6);
  }
}

.detail-block {
  max-height: 200px;
  overflow: hidden;
}

.header-gradient {
  height: 70px;
  width: 100%;
  margin-top: 130px;
  position: absolute;
  background: linear-gradient(0deg, #e8ecf1, transparent);
  z-index: 1;
}

.expanding-button {
  cursor: pointer;
}
</style>
