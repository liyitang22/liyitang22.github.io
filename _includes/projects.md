<h2 id="projects">Projects</h2>

<div class="project-list">
{% assign pinned_projects = site.data.projects.main | where: "pinned", true %}
{% assign dated_projects = site.data.projects.main | sort: "date" | reverse %}
{% for project in pinned_projects %}
  <article class="project-card{% if project.pinned %} pinned{% endif %}">
    <a class="project-figure" href="{{ project.url }}" target="_blank" rel="noopener">
      <img src="{{ project.image }}" alt="{{ project.title }} preview">
    </a>
    <div class="project-body">
      <div class="project-meta">
        {% if project.badge %}<span>{{ project.badge }}</span>{% endif %}
        <time>{{ project.date }}</time>
      </div>
      <h3><a href="{{ project.url }}" target="_blank" rel="noopener">{{ project.title }}</a></h3>
      <p>{{ project.subtitle }}</p>
      <a href="{{ project.url }}" target="_blank" rel="noopener">View project</a>
    </div>
  </article>
{% endfor %}
{% for project in dated_projects %}
  {% unless project.pinned %}
  <article class="project-card">
    <a class="project-figure" href="{{ project.url }}" target="_blank" rel="noopener">
      <img src="{{ project.image }}" alt="{{ project.title }} preview">
    </a>
    <div class="project-body">
      <div class="project-meta">
        {% if project.badge %}<span>{{ project.badge }}</span>{% endif %}
        <time>{{ project.date }}</time>
      </div>
      <h3><a href="{{ project.url }}" target="_blank" rel="noopener">{{ project.title }}</a></h3>
      <p>{{ project.subtitle }}</p>
      <a href="{{ project.url }}" target="_blank" rel="noopener">View project</a>
    </div>
  </article>
  {% endunless %}
{% endfor %}
</div>
