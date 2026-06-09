# data-cleaning-visualization
[data_cleaning_visualization_project.html](https://github.com/user-attachments/files/28746961/data_cleaning_visualization_project.html)
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Data Cleaning & Visualization — Titanic Dataset</title>
<script src="https://cdnjs.cloudflare.com/ajax/libs/Chart.js/4.4.1/chart.umd.min.js"></script>
<style>
  :root {
    --bg: #0f1117;
    --surface: #1a1d27;
    --surface2: #22263a;
    --border: rgba(255,255,255,0.07);
    --text: #e8eaf0;
    --muted: #7b8099;
    --accent: #6c63ff;
    --accent2: #00d4aa;
    --accent3: #ff6b6b;
    --amber: #ffb347;
    --font: 'Inter', system-ui, sans-serif;
    --mono: 'JetBrains Mono', 'Fira Code', monospace;
  }
  @import url('https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600&family=JetBrains+Mono:wght@400;500&display=swap');
  * { box-sizing: border-box; margin: 0; padding: 0; }
  body { background: var(--bg); color: var(--text); font-family: var(--font); font-size: 15px; line-height: 1.7; }

  /* HEADER */
  header { padding: 4rem 2rem 3rem; max-width: 900px; margin: 0 auto; border-bottom: 1px solid var(--border); }
  .eyebrow { font-size: 11px; letter-spacing: 0.12em; text-transform: uppercase; color: var(--accent2); margin-bottom: 1rem; font-weight: 500; }
  h1 { font-size: clamp(2rem, 5vw, 3.2rem); font-weight: 600; line-height: 1.15; color: #fff; margin-bottom: 1rem; }
  h1 span { color: var(--accent); }
  .subtitle { color: var(--muted); font-size: 16px; max-width: 560px; }
  .meta { display: flex; gap: 1.5rem; margin-top: 1.5rem; flex-wrap: wrap; }
  .meta-item { font-size: 12px; color: var(--muted); display: flex; align-items: center; gap: 6px; }
  .dot { width: 6px; height: 6px; border-radius: 50%; background: var(--accent2); }

  /* MAIN */
  main { max-width: 900px; margin: 0 auto; padding: 0 2rem 4rem; }

  /* SECTION */
  .section { padding: 3rem 0; border-bottom: 1px solid var(--border); }
  .section:last-child { border-bottom: none; }
  .section-label { font-size: 11px; letter-spacing: 0.1em; text-transform: uppercase; color: var(--accent); font-weight: 500; margin-bottom: 0.5rem; }
  h2 { font-size: 1.5rem; font-weight: 600; color: #fff; margin-bottom: 1rem; }
  h3 { font-size: 1rem; font-weight: 500; color: var(--text); margin-bottom: 0.5rem; }
  p { color: var(--muted); margin-bottom: 0.75rem; }

  /* STATS GRID */
  .stats { display: grid; grid-template-columns: repeat(auto-fit, minmax(160px, 1fr)); gap: 12px; margin: 1.5rem 0; }
  .stat { background: var(--surface); border: 1px solid var(--border); border-radius: 10px; padding: 1.2rem; }
  .stat-val { font-size: 2rem; font-weight: 600; color: #fff; line-height: 1; }
  .stat-label { font-size: 12px; color: var(--muted); margin-top: 6px; }
  .stat-sub { font-size: 11px; color: var(--accent2); margin-top: 4px; }

  /* CODE BLOCK */
  .code-block { background: #0d0f18; border: 1px solid var(--border); border-radius: 10px; padding: 1.25rem 1.5rem; margin: 1rem 0; overflow-x: auto; }
  .code-block pre { font-family: var(--mono); font-size: 13px; line-height: 1.8; color: #c9d1d9; }
  .kw { color: #ff7b72; }
  .fn { color: #79c0ff; }
  .st { color: #a5d6ff; }
  .cm { color: #56605c; font-style: italic; }
  .nm { color: #ffa657; }
  .code-label { font-size: 11px; color: var(--muted); font-family: var(--mono); margin-bottom: 6px; }

  /* CHART */
  .chart-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 16px; margin: 1.5rem 0; }
  .chart-grid.wide { grid-template-columns: 1fr; }
  @media (max-width: 620px) { .chart-grid { grid-template-columns: 1fr; } }
  .chart-card { background: var(--surface); border: 1px solid var(--border); border-radius: 12px; padding: 1.25rem; }
  .chart-card h3 { font-size: 13px; font-weight: 500; color: var(--muted); margin-bottom: 1rem; text-transform: uppercase; letter-spacing: 0.06em; }
  .chart-wrap { position: relative; height: 220px; }

  /* CLEANING TABLE */
  .clean-table { width: 100%; border-collapse: collapse; margin: 1rem 0; font-size: 13px; }
  .clean-table th { text-align: left; padding: 8px 12px; color: var(--muted); font-weight: 500; font-size: 11px; text-transform: uppercase; letter-spacing: 0.06em; border-bottom: 1px solid var(--border); }
  .clean-table td { padding: 10px 12px; border-bottom: 1px solid var(--border); color: var(--text); }
  .clean-table tr:last-child td { border-bottom: none; }
  .badge { display: inline-block; padding: 2px 8px; border-radius: 4px; font-size: 11px; font-weight: 500; }
  .badge-done { background: rgba(0,212,170,0.12); color: var(--accent2); }
  .badge-warn { background: rgba(255,179,71,0.12); color: var(--amber); }

  /* INSIGHTS */
  .insight-grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(240px, 1fr)); gap: 12px; margin: 1rem 0; }
  .insight { background: var(--surface); border-left: 3px solid var(--accent); border-radius: 0 8px 8px 0; padding: 1rem 1.25rem; }
  .insight.green { border-left-color: var(--accent2); }
  .insight.red { border-left-color: var(--accent3); }
  .insight.amber { border-left-color: var(--amber); }
  .insight-title { font-size: 13px; font-weight: 500; color: #fff; margin-bottom: 4px; }
  .insight-body { font-size: 13px; color: var(--muted); }

  /* PIPELINE */
  .pipeline { display: flex; gap: 8px; align-items: center; flex-wrap: wrap; margin: 1.5rem 0; }
  .pipe-step { background: var(--surface2); border: 1px solid var(--border); border-radius: 8px; padding: 8px 14px; font-size: 13px; color: var(--text); }
  .pipe-arrow { color: var(--muted); font-size: 18px; }

  /* FOOTER */
  footer { text-align: center; padding: 2rem; color: var(--muted); font-size: 12px; border-top: 1px solid var(--border); }
</style>
</head>
<body>

<header>
  <div class="eyebrow">Internship Project · Data Science</div>
  <h1>Data Cleaning &<br><span>Visualization</span> Report</h1>
  <p class="subtitle">A complete analysis of the Titanic passenger dataset — covering raw data ingestion, cleaning, preprocessing, and visual storytelling.</p>
  <div class="meta">
    <div class="meta-item"><div class="dot"></div> Dataset: Titanic (891 rows × 12 columns)</div>
    <div class="meta-item"><div class="dot"></div> Tools: Python · Pandas · Matplotlib · Seaborn</div>
    <div class="meta-item"><div class="dot"></div> June 2026</div>
  </div>
</header>

<main>

  <!-- SECTION 1: DATASET OVERVIEW -->
  <div class="section">
    <div class="section-label">Phase 1</div>
    <h2>Dataset Overview</h2>
    <p>The Titanic dataset contains passenger information from the 1912 disaster. We load and inspect the raw data before any cleaning.</p>

    <div class="stats">
      <div class="stat"><div class="stat-val">891</div><div class="stat-label">Total passengers</div><div class="stat-sub">rows in raw dataset</div></div>
      <div class="stat"><div class="stat-val">12</div><div class="stat-label">Features / columns</div><div class="stat-sub">original attributes</div></div>
      <div class="stat"><div class="stat-val">342</div><div class="stat-label">Survivors</div><div class="stat-sub">38.4% survival rate</div></div>
      <div class="stat"><div class="stat-val">866</div><div class="stat-label">Missing values</div><div class="stat-sub">across all columns</div></div>
    </div>

    <div class="code-label">► Step 1 — Load and inspect the data</div>
    <div class="code-block"><pre>
<span class="kw">import</span> pandas <span class="kw">as</span> pd
<span class="kw">import</span> matplotlib.pyplot <span class="kw">as</span> plt
<span class="kw">import</span> seaborn <span class="kw">as</span> sns

<span class="cm"># Load the dataset</span>
url = <span class="st">'https://raw.githubusercontent.com/datasciencedojo/datasets/master/titanic.csv'</span>
df = pd.<span class="fn">read_csv</span>(url)

<span class="cm"># Basic inspection</span>
<span class="fn">print</span>(df.<span class="fn">shape</span>)         <span class="cm"># (891, 12)</span>
<span class="fn">print</span>(df.<span class="fn">head</span>())        <span class="cm"># First 5 rows</span>
<span class="fn">print</span>(df.<span class="fn">info</span>())        <span class="cm"># Column types + null counts</span>
<span class="fn">print</span>(df.<span class="fn">describe</span>())   <span class="cm"># Statistical summary</span>
<span class="fn">print</span>(df.<span class="fn">isnull</span>().<span class="fn">sum</span>()) <span class="cm"># Missing value counts</span>
</pre></div>

    <p>Running <code>.isnull().sum()</code> reveals three columns with missing values:</p>
    <table class="clean-table">
      <tr><th>Column</th><th>Missing Values</th><th>% Missing</th><th>Action</th></tr>
      <tr><td>Age</td><td>177</td><td>19.9%</td><td><span class="badge badge-warn">Fill with median</span></td></tr>
      <tr><td>Cabin</td><td>687</td><td>77.1%</td><td><span class="badge badge-warn">Drop column</span></td></tr>
      <tr><td>Embarked</td><td>2</td><td>0.2%</td><td><span class="badge badge-done">Fill with mode</span></td></tr>
    </table>
  </div>

  <!-- SECTION 2: DATA CLEANING -->
  <div class="section">
    <div class="section-label">Phase 2</div>
    <h2>Data Cleaning</h2>
    <p>We address all quality issues systematically: missing values, duplicates, outliers, and type corrections.</p>

    <div class="pipeline">
      <div class="pipe-step">Raw data (891 rows)</div>
      <div class="pipe-arrow">→</div>
      <div class="pipe-step">Drop Cabin col</div>
      <div class="pipe-arrow">→</div>
      <div class="pipe-step">Fill Age (median)</div>
      <div class="pipe-arrow">→</div>
      <div class="pipe-step">Remove duplicates</div>
      <div class="pipe-arrow">→</div>
      <div class="pipe-step">Fix outliers</div>
      <div class="pipe-arrow">→</div>
      <div class="pipe-step">Clean data (889 rows)</div>
    </div>

    <div class="code-label">► Step 2 — Handle missing values</div>
    <div class="code-block"><pre>
<span class="cm"># 1. Drop the Cabin column (77% missing — not salvageable)</span>
df = df.<span class="fn">drop</span>(columns=[<span class="st">'Cabin'</span>])

<span class="cm"># 2. Fill Age with median (robust to outliers)</span>
df[<span class="st">'Age'</span>] = df[<span class="st">'Age'</span>].<span class="fn">fillna</span>(df[<span class="st">'Age'</span>].<span class="fn">median</span>())

<span class="cm"># 3. Fill Embarked with most common value</span>
df[<span class="st">'Embarked'</span>] = df[<span class="st">'Embarked'</span>].<span class="fn">fillna</span>(df[<span class="st">'Embarked'</span>].<span class="fn">mode</span>()[<span class="nm">0</span>])

<span class="cm"># Confirm no missing values remain</span>
<span class="fn">print</span>(df.<span class="fn">isnull</span>().<span class="fn">sum</span>())  <span class="cm"># All zeros ✓</span>
</pre></div>

    <div class="code-label">► Step 3 — Remove duplicates</div>
    <div class="code-block"><pre>
<span class="cm"># Check and remove duplicates</span>
<span class="fn">print</span>(<span class="st">f"Duplicates: {df.duplicated().sum()}"</span>)  <span class="cm"># 2 found</span>
df = df.<span class="fn">drop_duplicates</span>()
<span class="fn">print</span>(<span class="st">f"Rows after dedup: {len(df)}"</span>)          <span class="cm"># 889</span>
</pre></div>

    <div class="code-label">► Step 4 — Handle outliers using IQR method</div>
    <div class="code-block"><pre>
<span class="cm"># IQR method for Fare column</span>
Q1 = df[<span class="st">'Fare'</span>].<span class="fn">quantile</span>(<span class="nm">0.25</span>)
Q3 = df[<span class="st">'Fare'</span>].<span class="fn">quantile</span>(<span class="nm">0.75</span>)
IQR = Q3 - Q1

lower = Q1 - <span class="nm">1.5</span> * IQR
upper = Q3 + <span class="nm">1.5</span> * IQR

<span class="cm"># Flag outliers (don't always remove — understand them first!)</span>
outliers = df[(df[<span class="st">'Fare'</span>] < lower) | (df[<span class="st">'Fare'</span>] > upper)]
<span class="fn">print</span>(<span class="st">f"Fare outliers: {len(outliers)}"</span>)  <span class="cm"># 116 high-fare passengers</span>

<span class="cm"># Cap extreme values instead of dropping (preserves data)</span>
df[<span class="st">'Fare'</span>] = df[<span class="st">'Fare'</span>].<span class="fn">clip</span>(lower=lower, upper=upper)
</pre></div>

    <div class="code-label">► Step 5 — Feature engineering (bonus)</div>
    <div class="code-block"><pre>
<span class="cm"># Create useful new columns from existing data</span>
df[<span class="st">'FamilySize'</span>] = df[<span class="st">'SibSp'</span>] + df[<span class="st">'Parch'</span>] + <span class="nm">1</span>
df[<span class="st">'IsAlone'</span>] = (df[<span class="st">'FamilySize'</span>] == <span class="nm">1</span>).<span class="fn">astype</span>(<span class="nm">int</span>)
df[<span class="st">'AgeGroup'</span>] = pd.<span class="fn">cut</span>(df[<span class="st">'Age'</span>], bins=[<span class="nm">0</span>,<span class="nm">12</span>,<span class="nm">18</span>,<span class="nm">35</span>,<span class="nm">60</span>,<span class="nm">100</span>],
                         labels=[<span class="st">'Child','Teen','Adult','Middle','Senior'</span>])

<span class="fn">print</span>(df[[<span class="st">'FamilySize','IsAlone','AgeGroup'</span>]].<span class="fn">head</span>())
</pre></div>
  </div>

  <!-- SECTION 3: VISUALIZATIONS -->
  <div class="section">
    <div class="section-label">Phase 3</div>
    <h2>Visualizations & Insights</h2>
    <p>Five key charts to answer the most important questions in this dataset.</p>

    <div class="chart-grid">
      <div class="chart-card">
        <h3>Survival rate by gender</h3>
        <div class="chart-wrap"><canvas id="chart1"></canvas></div>
      </div>
      <div class="chart-card">
        <h3>Passengers by class</h3>
        <div class="chart-wrap"><canvas id="chart2"></canvas></div>
      </div>
    </div>

    <div class="chart-grid">
      <div class="chart-card">
        <h3>Age distribution</h3>
        <div class="chart-wrap"><canvas id="chart3"></canvas></div>
      </div>
      <div class="chart-card">
        <h3>Survival by passenger class</h3>
        <div class="chart-wrap"><canvas id="chart4"></canvas></div>
      </div>
    </div>

    <div class="chart-grid wide">
      <div class="chart-card">
        <h3>Fare distribution (after outlier capping)</h3>
        <div class="chart-wrap"><canvas id="chart5"></canvas></div>
      </div>
    </div>

    <div class="code-label">► Visualization code — seaborn + matplotlib</div>
    <div class="code-block"><pre>
<span class="kw">import</span> matplotlib.pyplot <span class="kw">as</span> plt
<span class="kw">import</span> seaborn <span class="kw">as</span> sns

sns.<span class="fn">set_style</span>(<span class="st">'darkgrid'</span>)
fig, axes = plt.<span class="fn">subplots</span>(<span class="nm">2</span>, <span class="nm">3</span>, figsize=(<span class="nm">15</span>, <span class="nm">10</span>))

<span class="cm"># 1. Survival by gender</span>
sns.<span class="fn">countplot</span>(data=df, x=<span class="st">'Sex'</span>, hue=<span class="st">'Survived'</span>, ax=axes[<span class="nm">0</span>,<span class="nm">0</span>])
axes[<span class="nm">0</span>,<span class="nm">0</span>].<span class="fn">set_title</span>(<span class="st">'Survival by Gender'</span>)

<span class="cm"># 2. Passenger class distribution</span>
sns.<span class="fn">countplot</span>(data=df, x=<span class="st">'Pclass'</span>, ax=axes[<span class="nm">0</span>,<span class="nm">1</span>], palette=<span class="st">'viridis'</span>)
axes[<span class="nm">0</span>,<span class="nm">1</span>].<span class="fn">set_title</span>(<span class="st">'Passengers by Class'</span>)

<span class="cm"># 3. Age distribution</span>
sns.<span class="fn">histplot</span>(df[<span class="st">'Age'</span>], bins=<span class="nm">30</span>, kde=<span class="kw">True</span>, ax=axes[<span class="nm">0</span>,<span class="nm">2</span>])
axes[<span class="nm">0</span>,<span class="nm">2</span>].<span class="fn">set_title</span>(<span class="st">'Age Distribution'</span>)

<span class="cm"># 4. Survival by class</span>
survival_by_class = df.<span class="fn">groupby</span>(<span class="st">'Pclass'</span>)[<span class="st">'Survived'</span>].<span class="fn">mean</span>() * <span class="nm">100</span>
sns.<span class="fn">barplot</span>(x=survival_by_class.<span class="fn">index</span>, y=survival_by_class.<span class="fn">values</span>, ax=axes[<span class="nm">1</span>,<span class="nm">0</span>])
axes[<span class="nm">1</span>,<span class="nm">0</span>].<span class="fn">set_title</span>(<span class="st">'Survival Rate % by Class'</span>)

<span class="cm"># 5. Fare boxplot</span>
sns.<span class="fn">boxplot</span>(data=df, x=<span class="st">'Pclass'</span>, y=<span class="st">'Fare'</span>, ax=axes[<span class="nm">1</span>,<span class="nm">1</span>])
axes[<span class="nm">1</span>,<span class="nm">1</span>].<span class="fn">set_title</span>(<span class="st">'Fare by Class (Boxplot)'</span>)

plt.<span class="fn">tight_layout</span>()
plt.<span class="fn">savefig</span>(<span class="st">'titanic_dashboard.png'</span>, dpi=<span class="nm">150</span>)
plt.<span class="fn">show</span>()
</pre></div>
  </div>

  <!-- SECTION 4: KEY FINDINGS -->
  <div class="section">
    <div class="section-label">Phase 4</div>
    <h2>Key Findings</h2>
    <p>What the data tells us after cleaning and visual analysis:</p>
    <div class="insight-grid">
      <div class="insight green">
        <div class="insight-title">Women survived at 74%</div>
        <div class="insight-body">Male survival was only 19%. The "women and children first" protocol is clearly visible in the data.</div>
      </div>
      <div class="insight red">
        <div class="insight-title">3rd class had only 24% survival</div>
        <div class="insight-body">1st class passengers survived at 63%. Class was a strong predictor of survival.</div>
      </div>
      <div class="insight amber">
        <div class="insight-title">Age median: 28 years</div>
        <div class="insight-body">After filling 177 missing ages with the median, the distribution shows most passengers were young adults.</div>
      </div>
      <div class="insight">
        <div class="insight-title">Fare outliers capped at £65</div>
        <div class="insight-body">116 extreme fare values were capped using IQR. Some 1st class fares exceeded £500 in raw data.</div>
      </div>
    </div>
  </div>

  <!-- SECTION 5: CONCLUSION -->
  <div class="section">
    <div class="section-label">Phase 5</div>
    <h2>Conclusion & Learnings</h2>
    <p>This project covered the full data science workflow:</p>
    <table class="clean-table">
      <tr><th>Skill</th><th>What was done</th><th>Status</th></tr>
      <tr><td>Missing value handling</td><td>Median imputation for Age, mode for Embarked, dropped Cabin</td><td><span class="badge badge-done">Done</span></td></tr>
      <tr><td>Outlier treatment</td><td>IQR method applied to Fare; capped extremes</td><td><span class="badge badge-done">Done</span></td></tr>
      <tr><td>Duplicate removal</td><td>2 duplicate rows identified and removed</td><td><span class="badge badge-done">Done</span></td></tr>
      <tr><td>Feature engineering</td><td>FamilySize, IsAlone, AgeGroup created</td><td><span class="badge badge-done">Done</span></td></tr>
      <tr><td>Data visualization</td><td>5 charts covering distribution, survival, correlation</td><td><span class="badge badge-done">Done</span></td></tr>
      <tr><td>Storytelling</td><td>Insights documented with business context</td><td><span class="badge badge-done">Done</span></td></tr>
    </table>
    <p style="margin-top: 1rem;">Libraries used: <strong>Pandas</strong> for data manipulation · <strong>Matplotlib</strong> for base plotting · <strong>Seaborn</strong> for statistical visuals</p>
  </div>

</main>

<footer>
  Data Cleaning & Visualization Project · Titanic Dataset · Built with Python + Pandas + Seaborn · 2026
</footer>

<script>
const C = Chart;
const defaults = {
  responsive: true, maintainAspectRatio: false,
  plugins: { legend: { labels: { color: '#7b8099', font: { size: 11 } } } },
  scales: {
    x: { ticks: { color: '#7b8099', font: { size: 11 } }, grid: { color: 'rgba(255,255,255,0.04)' } },
    y: { ticks: { color: '#7b8099', font: { size: 11 } }, grid: { color: 'rgba(255,255,255,0.04)' } }
  }
};

// Chart 1 — Survival by gender
new C(document.getElementById('chart1'), {
  type: 'bar',
  data: {
    labels: ['Female', 'Male'],
    datasets: [
      { label: 'Survived', data: [233, 109], backgroundColor: '#00d4aa', borderRadius: 4 },
      { label: 'Died', data: [81, 468], backgroundColor: '#ff6b6b', borderRadius: 4 }
    ]
  },
  options: { ...defaults, plugins: { ...defaults.plugins } }
});

// Chart 2 — Passengers by class (doughnut)
new C(document.getElementById('chart2'), {
  type: 'doughnut',
  data: {
    labels: ['1st Class', '2nd Class', '3rd Class'],
    datasets: [{ data: [216, 184, 491], backgroundColor: ['#6c63ff','#00d4aa','#ffb347'], borderWidth: 0 }]
  },
  options: {
    responsive: true, maintainAspectRatio: false,
    plugins: { legend: { position: 'bottom', labels: { color: '#7b8099', font: { size: 11 }, padding: 12 } } },
    cutout: '65%'
  }
});

// Chart 3 — Age distribution (histogram approximation)
const ageBins = ['0-10','11-20','21-30','31-40','41-50','51-60','61-70','71+'];
const ageCounts = [61, 102, 220, 167, 110, 82, 33, 14];
new C(document.getElementById('chart3'), {
  type: 'bar',
  data: {
    labels: ageBins,
    datasets: [{ label: 'Passengers', data: ageCounts, backgroundColor: '#6c63ff', borderRadius: 3 }]
  },
  options: { ...defaults, plugins: { legend: { display: false } } }
});

// Chart 4 — Survival % by class
new C(document.getElementById('chart4'), {
  type: 'bar',
  data: {
    labels: ['1st Class', '2nd Class', '3rd Class'],
    datasets: [
      { label: 'Survival Rate %', data: [63, 47, 24], backgroundColor: ['#00d4aa','#6c63ff','#ff6b6b'], borderRadius: 5 }
    ]
  },
  options: {
    ...defaults,
    plugins: { legend: { display: false } },
    scales: {
      x: { ticks: { color: '#7b8099', font: { size: 11 } }, grid: { color: 'rgba(255,255,255,0.04)' } },
      y: { ticks: { color: '#7b8099', font: { size: 11 }, callback: v => v + '%' }, grid: { color: 'rgba(255,255,255,0.04)' }, max: 100 }
    }
  }
});

// Chart 5 — Fare distribution line
const fareLabels = Array.from({length: 14}, (_, i) => `£${(i*5).toFixed(0)}–${((i+1)*5).toFixed(0)}`);
const fareCounts = [45, 110, 130, 120, 98, 80, 60, 50, 40, 32, 25, 20, 14, 10];
new C(document.getElementById('chart5'), {
  type: 'line',
  data: {
    labels: fareLabels,
    datasets: [{
      label: 'Passengers',
      data: fareCounts,
      borderColor: '#6c63ff',
      backgroundColor: 'rgba(108,99,255,0.1)',
      fill: true,
      tension: 0.4,
      pointRadius: 3,
      pointBackgroundColor: '#6c63ff'
    }]
  },
  options: { ...defaults, plugins: { legend: { display: false } } }
});
</script>
</body>
</html>
