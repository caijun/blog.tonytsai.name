---
title: Search
layout: page
---
<!-- Power on https://github.com/slashdotdash/jekyll-lunr-js-search-->

<script src="/media/js/search.min.js" type="text/javascript" charset="utf-8"></script>

<div id="search">
  <form action="/search" method="get">
    <input type="text" id="search-query" name="q" placeholder="" autocomplete="off">
  </form>
</div>

<section id="search-results" style="display: none;">
  <p>Search results:</p>
  <div class="entries">
  </div>
</section>

{% raw %}
<script id="search-results-template" type="text/mustache">
  {{#entries}}
    <article>
      <ul class="listing">
        {{#date}}<small><time datetime="{{pubdate}}" pubdate>{{pubdate}}</time></small>{{/date}}
        <a href="{{url}}">{{title}}</a>
      </ul>
    </article>
  {{/entries}}
</script>
{% endraw %}

<script type="text/javascript">
  $(function() {
    $('#search-query').lunrSearch({
      indexUrl: '/search.json',             // URL of the `search.json` index data for your site
      results:  '#search-results',          // jQuery selector for the search results container
      entries:  '.entries',                 // jQuery selector for the element to contain the results list, must be a child of the results element above.
      template: '#search-results-template'  // jQuery selector for the Mustache.js template
    });
  });
</script>