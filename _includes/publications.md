<h2 id="publications">Publications</h2>
<p class="github-project-summary pending" aria-live="polite"><i class="fa-solid fa-star" aria-hidden="true"></i> First-author projects: <strong id="github-project-star-total"></strong> stars across <span id="github-project-repo-count"></span> repositories</p>

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
    {% if link.video %}
    <video class="teaser img-fluid z-depth-1" width="270" height="123" autoplay muted loop playsinline preload="metadata" poster="{{ link.image }}" aria-label="{{ link.title }} preview">
      <source src="{{ link.video }}" type="video/mp4">
    </video>
    {% elsif link.image %}
    <img src="{{ link.image }}" class="teaser img-fluid z-depth-1" style="width=100;height=40%">
    {% endif %}
    {% if link.conference_short %} 
    <abbr class="badge">{{ link.conference_short }}</abbr>
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
      <a href="{{ link.code }}" class="btn btn-sm z-depth-0 github-stars pending" role="button" target="_blank" rel="noopener" style="font-size:12px;" data-github-url="{{ link.code }}" data-first-author="{{ link.first_author | default: false }}">
        <i class="fa-solid fa-star"></i> <span class="github-star-count"></span>
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
    if (!repo) {
      item.remove();
      return;
    }

    if (!repositories[repo]) repositories[repo] = { items: [], firstAuthor: false };
    repositories[repo].items.push(item);
    repositories[repo].firstAuthor = repositories[repo].firstAuthor || item.getAttribute('data-first-author') === 'true';
  });

  var repoNames = Object.keys(repositories);
  Promise.all(repoNames.map(function(repo) {
    var entry = repositories[repo];
    return fetch('https://api.github.com/repos/' + repo)
      .then(function(response) {
        if (!response.ok) throw new Error('GitHub API unavailable');
        return response.json();
      })
      .then(function(data) {
        var count = Number(data.stargazers_count);
        if (!Number.isFinite(count)) throw new Error('GitHub star count unavailable');
        entry.items.forEach(function(item) {
          item.querySelector('.github-star-count').textContent = formatStars(count);
          item.classList.remove('pending');
        });
        return { count: count, firstAuthor: entry.firstAuthor };
      })
      .catch(function() {
        entry.items.forEach(function(item) { item.remove(); });
        return null;
      });
  })).then(function(results) {
    var firstAuthorRepos = results.filter(function(result) { return result && result.firstAuthor; });
    if (!firstAuthorRepos.length) return;

    var total = firstAuthorRepos.reduce(function(sum, result) { return sum + result.count; }, 0);
    var totalElement = document.getElementById('github-project-star-total');
    var repoCountElement = document.getElementById('github-project-repo-count');
    var summary = document.querySelector('.github-project-summary');
    if (totalElement) totalElement.textContent = total.toLocaleString();
    if (repoCountElement) repoCountElement.textContent = String(firstAuthorRepos.length);
    if (summary) summary.classList.remove('pending');
  });
</script>
