<template>
<transition-group name="repo-list" tag="div">
  <div class="repo-item" :class="repo.isFilterOut ? 'bg-gray' : 'bg-white'" v-for="repo in repos" :key="repo.name">
    <!-- author avatar -->
    <a class=" mt-2 mr-2" :href="repo.author_url" target="_blank" rel="noopener">
      <img class="avatar" :class="repo.isFilterOut && 'img-gray'" :src="repo.avatar_url" width="48" height="48" :alt="`Avatar of ${repo.name}`">
    </a>
    <!-- repo info -->
    <div>
      <!-- repo name -->
      <h3>
        <a :class="repo.isFilterOut && 'text-gray'" :href="`https://github.com/${repo.name}`" target="_blank" rel="noopener">
          <span class="text-normal">{{ repo.authorName }} / </span>{{ repo.repoName }}
        </a>
      </h3>
      <!-- version tag name & publish date -->
      <a
        :href="repo.html_url" target="_blank" rel="noopener"
        :class="repo.isFresh && !repo.isFilterOut ? 'text-orange' : 'text-gray'"
      >
        <octicon v-once name="tag" flip="horizontal"></octicon>
        {{ repo.tag_name || 'no release' }}
        ~
        {{ repo.published_from_now }}
      </a>
    </div> <!-- repo info -->
    <!-- assets -->
    <div v-once class="assets">
      <!-- uploaded assets -->
      <span v-for="asset in repo.assets.slice(0, 8)" class="tooltipped tooltipped-nw tooltipped-no-delay ml-3" :aria-label="asset.name">
        <a class="asset text-gray" :href="asset.browser_download_url">
          <octicon :name="asset.icon_name" scale="2"></octicon>
        </a>
      </span> <!-- uploaded assets -->
      <!-- zipball & rarball -->
      <template v-if="repo.assets.length <= 6">
        <span v-if="repo.zipball_url" class="tooltipped tooltipped-nw tooltipped-no-delay ml-3" aria-label="Source code (zip)">
          <a class="asset text-gray-lighter" :href="repo.zipball_url">
            <octicon name="file-zip" scale="2"></octicon>
          </a>
        </span>
        <span v-if="repo.tarball_url" class="tooltipped tooltipped-nw tooltipped-no-delay ml-3" aria-label="Source code (tar.gz)">
          <a class="asset text-gray-lighter" :href="repo.tarball_url">
            <octicon name="file-zip" scale="2"></octicon>
          </a>
        </span>
      </template> <!-- zipball & rarball -->
    </div> <!-- assets -->
  </div> <!-- .repo-item -->
</transition-group>
</template>

<script>
export default {
  name: 'repo-list',
  props: ['repos'],
}
</script>


<style lang="scss" scoped>
.repo-item {
  display: flex;
  align-items: center;
  padding: 8px;
  border-bottom: 1px #e1e4e8 solid;

  .avatar {
    align-self: flex-start;
  }
}

.assets {
  display: flex;
  margin-left: auto;
}

.repo-list-move {
  transition: transform 0.9s;
}
</style>
