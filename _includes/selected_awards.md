## Selected Awards
<div class="section-tabs award-tabs" role="tablist" aria-label="Award years">
  <button class="section-tab award-tab active" type="button" data-category="2025" role="tab" aria-selected="true">2025</button>
  <button class="section-tab award-tab" type="button" data-category="2024" role="tab" aria-selected="false">2024</button>
  <button class="section-tab award-tab" type="button" data-category="2023" role="tab" aria-selected="false">2023</button>
  <button class="section-tab award-tab" type="button" data-category="2022" role="tab" aria-selected="false">2022</button>
</div>

<div class="section-panel award-panel active" data-category="2025">

<ul style="margin:0 0 5px;">
  <li><a><autocolor>Presidential Scholarship of Tsinghua University <span style="font-family: 'Microsoft YaHei', 'SimHei', sans-serif;">(特等奖学金)</span> </autocolor></a><strong> <i style="color:#e74d3c">10 undergraduate recipients university-wide</i></strong></li>
  <li><a><autocolor>Yao Award (Silver)</autocolor></a><strong> <i style="color:#e74d3c">Highest honor in Yao Class</i></strong></li>
</ul>
</div>

<div class="section-panel award-panel" data-category="2024">

<ul style="margin:0 0 5px;">
  <li><a><autocolor>National Scholarship.</autocolor></a><strong> <i style="color:#e74d3c">Highest honor for undergraduates</i></strong></li>
  <li><a><autocolor>SenseTime Scholarship.</autocolor></a></li>
  <li><a><autocolor>Golden Award Team for Summer Social Practice.</autocolor></a><strong> <i style="color:#e74d3c">Only 5 in the entire school</i></strong></li>
</ul>
</div>

<div class="section-panel award-panel" data-category="2023">

<ul style="margin:0 0 5px;">
  <li><a><autocolor>National Scholarship.</autocolor></a><strong> <i style="color:#e74d3c">Highest honor for undergraduates</i></strong></li>
  <li><a><autocolor>Outstanding Award for English Speech Contest.</autocolor></a></li>
</ul>
</div>

<div class="section-panel award-panel" data-category="2022">

<ul style="margin:0 0 5px;">
  <li><a><autocolor>Gold winner of the 22nd Asian Physics Olympiad.</autocolor></a></li>
  <li><a><autocolor>Theory winner & Best Girl of the 22nd Asian Physics Olympiad.</autocolor></a><strong> <i style="color:#e74d3c">Highest theory score</i></strong></li>
  <li><a><autocolor>Gold winner of the 38th Chinese Physics Olympics.</autocolor></a></li>
</ul>
</div>

<script>
  if (typeof initializeSectionTabs !== 'function') {
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
  }

  initializeSectionTabs('.award-tab', '.award-panel');
</script>
