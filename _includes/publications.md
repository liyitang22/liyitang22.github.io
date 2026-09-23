<h2 id="publications">Publications</h2>

<div class="publications">
{% assign publication_categories = "Humanoid Robots|Character Animation|Computer Vision" | split: "|" %}

<div class="section-tabs publication-tabs" role="tablist" aria-label="Publication categories">
{% for category in publication_categories %}
  {% assign category_id = category | downcase | replace: " ", "-" %}
  <button class="section-tab publication-tab{% if forloop.first %} active{% endif %}" type="button" data-category="{{ category_id }}" role="tab" aria-selected="{% if forloop.first %}true{% else %}false{% endif %}">
    {{ category }}
  </button>
{% endfor %}
</div>

{% for category in publication_categories %}
{% assign category_id = category | downcase | replace: " ", "-" %}
<div class="section-panel publication-panel{% if forloop.first %} active{% endif %}" data-category="{{ category_id }}">
<ol class="bibliography">

{% assign categorized_publications = site.data.publications.main | where: "category", category %}
{% for link in categorized_publications %}

<li>
<div class="pub-row">
  <div class="col-sm-3 abbr" style="position: relative;padding-right: 15px;padding-left: 15px;">
    {% if link.image %} 
    <img src="{{ link.image }}" class="teaser img-fluid z-depth-1" style="width=100;height=40%">
    {% if link.conference_short %} 
    <abbr class="badge">{{ link.conference_short }}</abbr>
    {% endif %}
    {% endif %}
  </div>
  <div class="col-sm-9" style="position: relative;padding-right: 15px;padding-left: 20px;">
      {% assign title_url = link.pdf | default: link.project | default: link.code %}
      <div class="title"><a href="{{ title_url }}">{{ link.title }}</a></div>
      <div class="author">{{ link.authors }}</div>
      <div class="periodical"><em>{{ link.conference }}</em>
      </div>
    <div class="links">
      {% if link.pdf %} 
      <a href="{{ link.pdf }}" class="btn btn-sm z-depth-0" role="button" target="_blank" style="font-size:12px;">PDF</a>
      {% endif %}
      {% if link.code %} 
      <a href="{{ link.code }}" class="btn btn-sm z-depth-0" role="button" target="_blank" style="font-size:12px;">Code</a>
      {% endif %}
      {% if link.github_stars %}
      <a href="{{ link.code }}" class="btn btn-sm z-depth-0" role="button" target="_blank" style="font-size:12px;"><i class="fa-solid fa-star"></i> {{ link.github_stars }}</a>
      {% endif %}
      {% if link.page %} 
      <a href="{{ link.page }}" class="btn btn-sm z-depth-0" role="button" target="_blank" style="font-size:12px;">Project Page</a>
      {% endif %}
      {% if link.project %} 
      <a href="{{ link.project }}" class="btn btn-sm z-depth-0" role="button" target="_blank" style="font-size:12px;">Project</a>
      {% endif %}
      {% if link.notes %} 
      <strong> <i style="color:#e74d3c">{{ link.notes }}</i></strong>
      {% endif %}
      {% if link.others %} 
      {{ link.others }}
      {% endif %}
    </div>
  </div>
</div>
</li>
<br>

{% endfor %}

</ol>
</div>
{% endfor %}
</div>

<script>
  function initializeSectionTabs(tabSelector, panelSelector) {
    document.querySelectorAll(tabSelector).forEach(function(tab) {
      tab.addEventListener('click', function() {
        var selectedCategory = tab.getAttribute('data-category');
        document.querySelectorAll(tabSelector).forEach(function(item) {
          var isActive = item === tab;
          item.classList.toggle('active', isActive);
          item.setAttribute('aria-selected', isActive ? 'true' : 'false');
        });
        document.querySelectorAll(panelSelector).forEach(function(panel) {
          panel.classList.toggle('active', panel.getAttribute('data-category') === selectedCategory);
        });
      });
    });
  }

  initializeSectionTabs('.publication-tab', '.publication-panel');
</script>
