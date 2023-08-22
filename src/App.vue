<template>
  <div class="container">
    <div id="autocomplete" />
  </div>
</template>

<script lang="jsx">
import { h, Fragment, render, onMounted } from 'vue';
import { autocomplete, getAlgoliaResults } from '@algolia/autocomplete-js';
import algoliasearch from 'algoliasearch/lite';
import { createQuerySuggestionsPlugin } from '@algolia/autocomplete-plugin-query-suggestions';

import '@algolia/autocomplete-theme-classic';

import { createElement } from './utils/createElement';
import ProductItem from './components/ProductItem.vue';

const appId = 'P4ON113IHW';
const apiKey = 'bcf227a1226bc6aefd94ed1f891a37c7';
const searchClient = algoliasearch(appId, apiKey);

const querySuggestionPluginInCategory = createQuerySuggestionsPlugin({
  searchClient,
  indexName: 'TestingAlgolia_query_suggestions',
  transformSource({ source }) {
    return {
      ...source,
      onSelect({ setIsOpen }) {
        setIsOpen(true);
      },
    };
  },
  });



export default {
  name: 'App',
  setup() {
    onMounted(() => {
      autocomplete({
        plugins: [querySuggestionPluginInCategory],
        container: '#autocomplete',
        placeholder: 'Search',
        getSources({ query }) {
          return [
            {
              sourceId: 'products',
              getItems() {
                return getAlgoliaResults({
                  searchClient,
                  queries: [
                    {
                      indexName: 'TestingAlgolia',
                      query,
                    },
                  ],
                });
              },
              templates: {
                item({ item, components }) {
                  return (
                    <ProductItem item={item} highlight={components.Highlight} />
                  );
                },
              },
            },
          ];
        },
        renderer: {
          createElement,
          Fragment,
          render,
        },
      });
    });
  },
};
</script>

<style>
.container {
  margin: 0 auto;
  max-width: 640px;
  width: 100%;
}
</style>