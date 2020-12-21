---
# Feel free to add content and custom Front Matter to this file.
# To modify the layout, see https://jekyllrb.com/docs/themes/#overriding-theme-defaults

layout: default
---
<script src="https://cdn.jsdelivr.net/npm/instantsearch.js@2.2.1/dist/instantsearch.min.js"></script>
<input id="search-box">
<p>Data provided by <strong>Oddmeow/Alluyn</strong>, should theoretically be getting updated every hour.</p>
<hr class="break">

<section class="section">
  <div class="container">

    <div class="container columns">
      <div class="column is-one-third">
        <h3 class="is-size-4 filter-header">Filter Controls</h3>
        <div id="rev-box"></div>
        <div id="refine"></div>
        <div id="location"></div>
      </div>
      <div class="column">
        <div id="hits">
          <div class="spinner">
            <div class="bounce1"></div>
            <div class="bounce2"></div>
            <div class="bounce3"></div>
          </div>
        </div>
      </div>
    </div>
  </div>
</section>

{% raw %}
<script type="text/javascript">
  function getUrlParameter(name) {
    name = name.replace(/[\[]/, '\\[').replace(/[\]]/, '\\]');
    var regex = new RegExp('[\\?&]' + name + '=([^&#]*)');
    var results = regex.exec(location.search);
    return results === null ? '' : decodeURIComponent(results[1].replace(/\+/g, ' '));
  };

  const search = instantsearch({
    appId: 'R7MRY12BR6',
    apiKey: 'eb697a1a5ada3cbf96bc9279f540c0e6',
    indexName: 'shopper',
    searchParameters: {
      attributesToSnippet: ["name:30", "datadump:60"],
      facetingAfterDistinct: true,
      query: "",
      snippetEllipsisText: '[&hellip;]'
    }
  });

  search.addWidget(
    instantsearch.widgets.searchBox({
      container: '#search-box',
      placeholder: 'Search Player Shops',
      autofocus: 'true'
    })
  );

  search.addWidget(
    instantsearch.widgets.menu({
      container: '#rev-box',
      attributeName: 'location',
      operator: 'or',
      limit: 3,
      sortBy: ["count:desc", "name:asc"],
      templates: {
        header: '<h3 class="is-size-5 mb-3" style="display: block;">Store Location</h3>',
        item: '<button class="button is-outlined is-link">{{ label }}<span class="count"> ({{ count }} items)</span></button>'
      },
      transformData: function (item) {
        
        return item;
      }
    })
  );

  search.addWidget(
    instantsearch.widgets.infiniteHits({
      container: '#hits',
      templates: {
        empty: 'No results',
        item: '<h3 class="is-size-5 is-capitalized">{{{_highlightResult.name.value}}}</h3><span class="alg-text">{{#datadump}}{{{_snippetResult.datadump.value}}}{{/datadump}}{{^datadump}}{{{_snippetResult.datadump.value}}}{{/datadump}}</span><br><p class="item-details"><strong>Shop:</strong> {{shop}} |<strong> Price:</strong> {{price}} | <strong> Shop Number:</strong> {{shopnum}}</p><hr>'
      }
    })
  );

  search.addWidget(
    instantsearch.widgets.refinementList({
      container: '#refine',
      attributeName: 'enchant',
      operator: 'or',
      limit: 10,
      transformData: function (item) {
        return item;
      },
      templates: {
        header: '<h3 class="is-size-5 mb-3" style="display: block;">Enchant</h3>',
        item: '<button class="button is-outlined is-link">+{{label}} <span class="count"> ({{ count }} items)</span></button>'
      }
    })
  );
  search.addWidget(
      instantsearch.widgets.refinementList({
        container: '#location',
        attributeName: 'wornlocation',
        operator: 'or',
        limit: 10,
        transformData: function (item) {
          return item;
        },
        templates: {
          header: '<h3 class="is-size-5 mb-3" style="display: block;">Worn Location</h3>',
          item: '<button class="button is-outlined is-link">{{label}} <span class="count"> ({{ count }} items)</span></button>'
        }
      })
    );


  search.start();

</script>
{% endraw %}