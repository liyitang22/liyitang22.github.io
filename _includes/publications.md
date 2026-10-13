<h2 id="publications">Publications</h2>
<p class="github-project-summary" aria-live="polite"><i class="fa-solid fa-star" aria-hidden="true"></i> GitHub projects: <strong id="github-project-star-total">2,073</strong> stars across <span id="github-project-repo-count">9</span> repositories</p>

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
      {% if link.code and link.code contains "github.com" %}
      <a href="{{ link.code }}" class="btn btn-sm z-depth-0 github-stars{% unless link.github_stars %} pending{% endunless %}" role="button" target="_blank" style="font-size:12px;" data-github-url="{{ link.code }}" data-fallback-stars="{{ link.github_stars | default: '' }}">
        <i class="fa-solid fa-star"></i> <span class="github-star-count">{% if link.github_stars %}{{ link.github_stars }}{% else %}...{% endif %}</span>
      </a>
      {% endif %}
      {% if link.page %} 
      <a href="{{ link.page }}" class="btn btn-sm z-depth-0" role="button" target="_blank" style="font-size:12px;">Project Page</a>
      {% endif %}
      {% if link.project %} 
      <a href="{{ link.project }}" class="btn btn-sm z-depth-0" role="button" target="_blank" style="font-size:12px;">Project</a>
      {% endif %}
      {% if link.huggingface %}
      <a href="{{ link.huggingface }}" class="btn btn-sm z-depth-0" role="button" target="_blank" rel="noopener" style="font-size:12px;">Hugging Face</a>
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

  function parseGitHubRepo(url) {
    try {
      var parsed = new URL(url);
      if (parsed.hostname !== 'github.com') return null;
      var parts = parsed.pathname.split('/').filter(Boolean);
      if (parts.length < 2) return null;
      return parts[0] + '/' + parts[1];
    } catch (error) {
      return null;
    }
  }

  function formatStars(count) {
    if (count >= 1000) {
      return (Math.round(count / 100) / 10).toString().replace(/\.0$/, '') + 'k';
    }
    return String(count);
  }

  var repositories = {};

  document.querySelectorAll('.github-stars').forEach(function(item) {
    var repo = parseGitHubRepo(item.getAttribute('data-github-url'));
    var fallback = parseInt(item.getAttribute('data-fallback-stars'), 10);
    if (!repo) {
      item.remove();
      return;
    }

    item.classList.remove('pending');
    if (Number.isFinite(fallback)) {
      item.querySelector('.github-star-count').textContent = formatStars(fallback);
    }

    if (!repositories[repo]) repositories[repo] = { items: [], fallback: fallback };
    repositories[repo].items.push(item);
  });

  var repoNames = Object.keys(repositories);
  Promise.all(repoNames.map(function(repo) {
    var entry = repositories[repo];
    return fetch('https://api.github.com/repos/' + repo)
      .then(function(response) {
        if (!response.ok) throw new Error('GitHub API unavailable');
        return response.json();
      })
      .then(function(data) { return Number(data.stargazers_count); })
      .catch(function() { return entry.fallback; })
      .then(function(count) {
        if (Number.isFinite(count)) {
          entry.items.forEach(function(item) {
            item.querySelector('.github-star-count').textContent = formatStars(count);
          });
          return count;
        }
        return null;
      });
  })).then(function(counts) {
    var availableCounts = counts.filter(Number.isFinite);
    var total = availableCounts.reduce(function(sum, count) { return sum + count; }, 0);
    var totalElement = document.getElementById('github-project-star-total');
    var repoCountElement = document.getElementById('github-project-repo-count');
    if (totalElement) totalElement.textContent = total.toLocaleString();
    if (repoCountElement) repoCountElement.textContent = String(availableCounts.length);
  });
</script>
