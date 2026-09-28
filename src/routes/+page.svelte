<script>
  import { onMount } from 'svelte';

  const SAMPLE_LOGS = {
    apache: `192.168.1.10 - - [29/Sep/2026:10:21:34 +0530] "GET /login HTTP/1.1" 200 532
192.168.1.15 - - [29/Sep/2026:10:21:36 +0530] "POST /login HTTP/1.1" 401 128
192.168.1.22 - - [29/Sep/2026:10:21:41 +0530] "GET /dashboard HTTP/1.1" 200 1042
192.168.1.30 - - [29/Sep/2026:10:22:04 +0530] "GET /admin HTTP/1.1" 404 321
192.168.1.45 - - [29/Sep/2026:10:22:18 +0530] "POST /api/v1/checkout HTTP/1.1" 201 842
10.0.0.14 - - [29/Sep/2026:10:23:02 +0530] "GET /static/css/main.css HTTP/1.1" 200 4096
10.0.0.18 - - [29/Sep/2026:10:23:15 +0530] "GET /static/js/bundle.js HTTP/1.1" 200 48201
172.16.0.5 - - [29/Sep/2026:10:24:00 +0530] "DELETE /api/users/89 HTTP/1.1" 403 76
192.168.1.105 - - [29/Sep/2026:10:24:22 +0530] "GET /search?q=cybersecurity HTTP/1.1" 200 2154
10.0.0.99 - - [29/Sep/2026:10:25:01 +0530] "PUT /api/settings HTTP/1.1" 200 321
192.168.1.30 - - [29/Sep/2026:10:25:30 +0530] "GET /admin/config.php HTTP/1.1" 200 512
192.168.1.12 - - [29/Sep/2026:10:26:14 +0530] "GET /health HTTP/1.1" 200 17`,

    ssh: `Sep 29 10:31:02 server sshd[1234]: Failed password for admin from 192.168.1.20 port 42132
Sep 29 10:31:08 server sshd[1235]: Accepted password for anshum from 192.168.1.15 port 42141
Sep 29 10:31:19 server sshd[1236]: Failed password for root from 10.0.0.12 port 43121
Sep 29 10:31:25 server sshd[1237]: Failed password for invalid user oracle from 203.0.113.45 port 55122
Sep 29 10:31:40 server sshd[1238]: Failed password for invalid user test from 203.0.113.45 port 55198
Sep 29 10:32:01 server sshd[1239]: Accepted publickey for deployer from 192.168.1.50 port 38910
Sep 29 10:32:15 server sshd[1240]: Connection closed by authenticating user root 198.51.100.14 port 49102 [preauth]
Sep 29 10:32:44 server sshd[1241]: Failed password for root from 10.0.0.12 port 43190
Sep 29 10:33:02 server sshd[1242]: Accepted password for anshum from 192.168.1.15 port 42200
Sep 29 10:33:18 server sshd[1243]: Invalid user guest from 198.51.100.77 port 39820
Sep 29 10:34:05 server sshd[1244]: Received disconnect from 203.0.113.45 port 55301: 11: Bye Bye [preauth]
Sep 29 10:35:10 server sshd[1245]: Accepted password for backup from 192.168.1.80 port 41209`,

    application: `2026-09-29 10:42:11 ERROR User login failed user=admin ip=10.0.0.15 reason=bad_password
2026-09-29 10:42:19 INFO User login successful user=anshum ip=10.0.0.20 session=sess_9821
2026-09-29 10:43:01 WARN Invalid token user=guest ip=10.0.0.30 endpoint=/api/v2/reports
2026-09-29 10:43:15 INFO Password reset requested user=alice ip=192.168.1.99 token=rst_3391
2026-09-29 10:43:40 ERROR Database timeout on query latency=4502ms host=db-primary
2026-09-29 10:44:05 WARN Rate limit exceeded user=crawler_bot ip=198.51.100.90 hits=120
2026-09-29 10:44:22 INFO Payment processed order_id=ord_9901 amount=149.99 status=success
2026-09-29 10:44:59 ERROR Out of memory error in worker pool worker_id=wk_04
2026-09-29 10:45:12 INFO User logout user=anshum ip=10.0.0.20 duration=173s
2026-09-29 10:45:45 WARN Suspicious header detected user=attacker ip=203.0.113.19 agent="sqlmap/1.5"
2026-09-29 10:46:01 INFO Cache cleared by admin user=admin ip=10.0.0.15 keys=450
2026-09-29 10:46:33 DEBUG Health check ping from load balancer ip=10.0.0.1 status=ok`,

    firewall: `Sep 29 10:50:01 firewall-edge kernel: [UFW BLOCK] IN=eth0 OUT= MAC=52:54:00:12:34:56:52:54:00:ab:cd:ef:08:00 SRC=203.0.113.88 DST=192.168.1.5 LEN=40 TOS=0x00 PREC=0x00 TTL=242 ID=49120 PROTO=TCP SPT=48192 DPT=22 WINDOW=1024 RES=0x00 SYN URGP=0
Sep 29 10:50:04 firewall-edge kernel: [UFW BLOCK] IN=eth0 OUT= MAC=52:54:00:12:34:56:52:54:00:ab:cd:ef:08:00 SRC=203.0.113.88 DST=192.168.1.5 LEN=40 TOS=0x00 PREC=0x00 TTL=242 ID=49121 PROTO=TCP SPT=48193 DPT=23 WINDOW=1024 RES=0x00 SYN URGP=0
Sep 29 10:50:07 firewall-edge kernel: [UFW BLOCK] IN=eth0 OUT= MAC=52:54:00:12:34:56:52:54:00:ab:cd:ef:08:00 SRC=203.0.113.88 DST=192.168.1.5 LEN=40 TOS=0x00 PREC=0x00 TTL=242 ID=49122 PROTO=TCP SPT=48194 DPT=80 WINDOW=1024 RES=0x00 SYN URGP=0
Sep 29 10:50:11 firewall-edge kernel: [UFW ALLOW] IN=eth0 OUT= MAC=52:54:00:12:34:56:52:54:00:ab:cd:ef:08:00 SRC=198.51.100.22 DST=192.168.1.5 LEN=60 TOS=0x00 PREC=0x00 TTL=64 ID=12901 PROTO=TCP SPT=54210 DPT=443 WINDOW=29200 RES=0x00 SYN URGP=0
Sep 29 10:50:19 firewall-edge kernel: [UFW BLOCK] IN=eth0 OUT= MAC=52:54:00:12:34:56:52:54:00:ab:cd:ef:08:00 SRC=192.0.2.140 DST=192.168.1.5 LEN=28 TOS=0x00 PREC=0x00 TTL=112 ID=3102 PROTO=ICMP TYPE=8 CODE=0
Sep 29 10:50:35 firewall-edge kernel: [UFW BLOCK] IN=eth0 OUT= MAC=52:54:00:12:34:56:52:54:00:ab:cd:ef:08:00 SRC=203.0.113.104 DST=192.168.1.5 LEN=48 TOS=0x00 PREC=0x00 TTL=50 ID=6019 PROTO=UDP SPT=53412 DPT=53 LEN=28
Sep 29 10:50:49 firewall-edge kernel: [UFW ALLOW] IN=eth0 OUT= MAC=52:54:00:12:34:56:52:54:00:ab:cd:ef:08:00 SRC=192.168.1.100 DST=8.8.8.8 LEN=56 TOS=0x00 PREC=0x00 TTL=64 ID=44201 PROTO=UDP SPT=60111 DPT=53 LEN=36
Sep 29 10:51:02 firewall-edge kernel: [UFW BLOCK] IN=eth0 OUT= MAC=52:54:00:12:34:56:52:54:00:ab:cd:ef:08:00 SRC=198.51.100.200 DST=192.168.1.5 LEN=40 TOS=0x00 PREC=0x00 TTL=239 ID=5510 PROTO=TCP SPT=61099 DPT=3389 WINDOW=1024 RES=0x00 SYN URGP=0
Sep 29 10:51:20 firewall-edge kernel: [UFW ALLOW] IN=eth0 OUT= MAC=52:54:00:12:34:56:52:54:00:ab:cd:ef:08:00 SRC=192.168.1.15 DST=192.168.1.5 LEN=52 TOS=0x00 PREC=0x00 TTL=64 ID=8910 PROTO=TCP SPT=39201 DPT=22 WINDOW=65535 RES=0x00 SYN URGP=0
Sep 29 10:51:44 firewall-edge kernel: [UFW BLOCK] IN=eth0 OUT= MAC=52:54:00:12:34:56:52:54:00:ab:cd:ef:08:00 SRC=203.0.113.88 DST=192.168.1.5 LEN=40 TOS=0x00 PREC=0x00 TTL=242 ID=49129 PROTO=TCP SPT=48201 DPT=8080 WINDOW=1024 RES=0x00 SYN URGP=0`
  };

  const CHALLENGES = [
    { id: 1, title: '01 Extract IP', hint: 'Extract client IP address' },
    { id: 2, title: '02 Add timestamp', hint: 'Add date and time capture group' },
    { id: 3, title: '03 Parse request', hint: 'Extract method and request path' },
    { id: 4, title: '04 Extract all fields', hint: 'Extract status code and response size' }
  ];

  // State
  let selectedSource = 'apache';
  let rawLog = SAMPLE_LOGS.apache;
  let activeChallenge = null;
  let showGeneratedRegex = true;
  let copied = false;
  let fileInputElement;

  // Initial Fields: start blank with no default input
  let fields = [
    { name: '', pattern: '' }
  ];

  // Compute generated regex dynamically or from sample full regex
  $: generatedRegex = buildGeneratedRegex(fields);
  $: parsed = parseLogData(rawLog, fields, generatedRegex);

  function buildGeneratedRegex(fieldList) {
    const valid = fieldList.filter(f => f.name.trim() !== '' && f.pattern.trim() !== '');
    if (valid.length === 0) return '';
    return valid
      .map(f => `(?<${cleanGroupName(f.name)}>${f.pattern.trim()})`)
      .join('.*?');
  }

  function cleanGroupName(name) {
    return name.trim().replace(/[^a-zA-Z0-9_]/g, '_');
  }

  function addField() {
    fields = [...fields, { name: '', pattern: '' }];
  }

  function removeField(index) {
    if (fields.length <= 1) {
      fields = [{ name: '', pattern: '' }];
    } else {
      fields = fields.filter((_, i) => i !== index);
    }
  }

  function handleSourceChange(event) {
    const val = event.target.value;
    selectedSource = val;
    if (SAMPLE_LOGS[val]) {
      rawLog = SAMPLE_LOGS[val];
    }
  }

  function handleFileUpload(event) {
    const file = event.target.files?.[0];
    if (!file) return;

    const reader = new FileReader();
    reader.onload = (e) => {
      rawLog = e.target.result;
      selectedSource = 'custom';
    };
    reader.readAsText(file);
    event.target.value = '';
  }

  function updateLogLine(index, newText) {
    const lines = rawLog.split(/\r?\n/);
    lines[index] = newText;
    rawLog = lines.join('\n');
  }

  function selectChallenge(id) {
    activeChallenge = id;
    if (id === 1) {
      fields = [{ name: 'ip', pattern: '\\d+\\.\\d+\\.\\d+\\.\\d+' }];
    } else if (id === 2) {
      fields = [
        { name: 'ip', pattern: '\\d+\\.\\d+\\.\\d+\\.\\d+' },
        { name: 'timestamp', pattern: '\\d{2}/[A-Za-z]+\\/\\d{4}:\\d{2}:\\d{2}:\\d{2}\\s[+-]\\d{4}' }
      ];
    } else if (id === 3) {
      fields = [
        { name: 'ip', pattern: '\\d+\\.\\d+\\.\\d+\\.\\d+' },
        { name: 'method', pattern: 'GET|POST|PUT|DELETE|PATCH|HEAD|OPTIONS' },
        { name: 'path', pattern: '/[^\\s]+' }
      ];
    } else if (id === 4) {
      fields = [
        { name: 'ip', pattern: '\\d+\\.\\d+\\.\\d+\\.\\d+' },
        { name: 'timestamp', pattern: '\\d{2}/[A-Za-z]+\\/\\d{4}:\\d{2}:\\d{2}:\\d{2}\\s[+-]\\d{4}' },
        { name: 'method', pattern: 'GET|POST|PUT|DELETE|PATCH|HEAD|OPTIONS' },
        { name: 'path', pattern: '/[^\\s]+' },
        { name: 'protocol', pattern: '[^"]+' },
        { name: 'status', pattern: '\\d{3}' },
        { name: 'size', pattern: '\\d+' }
      ];
    }
  }

  function parseLogData(logText, fieldList, genRegexStr) {
    const rawLines = logText ? logText.split(/\r?\n/) : [];
    const lines = rawLines.filter((l) => l.trim().length > 0);

    const validFields = fieldList
      .filter(f => f.name.trim() !== '' && f.pattern.trim() !== '')
      .map(f => {
        const cName = cleanGroupName(f.name);
        try {
          return {
            name: cName,
            reg: new RegExp(`(?<${cName}>${f.pattern.trim()})`)
          };
        } catch {
          return null;
        }
      })
      .filter(Boolean);

    if (validFields.length === 0) {
      return { lines, results: [], totalLines: lines.length, matchedCount: 0 };
    }

    const results = [];
    const fieldFirstMatch = {};

    for (const line of lines) {
      const rowData = {};
      let matchedAny = false;
      for (const f of validFields) {
        const m = f.reg.exec(line);
        if (m && m.groups && m.groups[f.name] !== undefined) {
          rowData[f.name] = m.groups[f.name];
          matchedAny = true;
          if (!fieldFirstMatch[f.name]) {
            fieldFirstMatch[f.name] = m.groups[f.name];
          }
        }
      }
      if (matchedAny) {
        results.push(rowData);
      }
    }

    return {
      lines,
      results,
      totalLines: lines.length,
      matchedCount: results.length,
      fieldFirstMatch
    };
  }

  function copyRegex() {
    if (!generatedRegex) return;
    navigator.clipboard.writeText(generatedRegex);
    copied = true;
    setTimeout(() => {
      copied = false;
    }, 2000);
  }

  function downloadJSON() {
    if (!parsed || !parsed.results) return;
    const blob = new Blob([JSON.stringify(parsed.results, null, 2)], {
      type: 'application/json'
    });
    const url = URL.createObjectURL(blob);
    const a = document.createElement('a');
    a.href = url;
    a.download = 'parsed-logs.json';
    a.click();
    URL.revokeObjectURL(url);
  }
</script>

<svelte:head>
  <title>LOGPARSER - Regex Lab</title>
</svelte:head>

<div class="app-root">
  <!-- Top Header -->
  <header class="header">
    <div class="header-left">
      <div class="logo-row">
        <h1 class="logo-title">LOGPARSER</h1>
        <span class="badge-regex-lab">REGEX LAB</span>
      </div>
      <p class="logo-subtitle">Turn raw logs into structured data using Regular Expressions</p>
    </div>

    <!-- Stepper Pipeline Pill -->
    <div class="pipeline-stepper">
      <div class="step-pill">Raw Log</div>
      <span class="step-arrow">→</span>
      <div class="step-pill">Regex</div>
      <span class="step-arrow">→</span>
      <div class="step-pill">Named Groups</div>
      <span class="step-arrow">→</span>
      <div class="step-pill">JSON</div>
    </div>
  </header>

  <!-- Controls Bar -->
  <div class="controls-bar">
    <div class="controls-left">
      <span class="label-heading">LOG SOURCE</span>
      <div class="select-wrapper">
        <select value={selectedSource} on:change={handleSourceChange} class="source-select">
          <option value="apache">Apache Access Log (12 lines)</option>
          <option value="ssh">SSH Auth Server Log (12 lines)</option>
          <option value="application">Application Events Log (12 lines)</option>
          <option value="firewall">UFW Firewall Log (10 lines)</option>
          {#if selectedSource === 'custom'}
            <option value="custom">Uploaded Custom Log</option>
          {/if}
        </select>
        <span class="select-caret">⌄</span>
      </div>

      <input
        type="file"
        accept=".log,.txt,text/plain"
        bind:this={fileInputElement}
        on:change={handleFileUpload}
        class="hidden-file-input"
        id="file-upload"
      />
      <button class="btn-upload" on:click={() => fileInputElement.click()}>
        <span class="upload-icon">↑</span> Upload Log (.log / .txt)
      </button>
    </div>
  </div>

  <!-- Dual Panels: RAW LOG & PARSED JSON -->
  <main class="panes-grid">
    <!-- Left Pane: RAW LOG -->
    <section class="card-pane">
      <div class="card-header">
        <div class="card-title-group">
          <svg class="pane-icon" width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
            <path d="M14 2H6a2 2 0 0 0-2 2v16a2 2 0 0 0 2 2h12a2 2 0 0 0 2-2V8z"></path>
            <polyline points="14 2 14 8 20 8"></polyline>
            <line x1="16" y1="13" x2="8" y2="13"></line>
            <line x1="16" y1="17" x2="8" y2="17"></line>
          </svg>
          <span class="card-title">RAW LOG</span>
        </div>
        <span class="card-meta">{parsed.totalLines} lines</span>
      </div>

      <div class="log-entries-scroll" role="textbox" aria-label="Raw Logs Editor">
        {#each parsed.lines as lineText, index}
          <div class="log-line-row">
            <span class="log-line-num">{index + 1}</span>
            <div
              class="log-line-content"
              contenteditable="true"
              spellcheck="false"
              on:input={(e) => updateLogLine(index, e.currentTarget.innerText)}
            >{lineText}</div>
          </div>
        {/each}
      </div>
    </section>

    <!-- Right Pane: PARSED JSON -->
    <section class="card-pane">
      <div class="card-header">
        <div class="card-title-group">
          <svg class="pane-icon blue" width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
            <path d="M16 18l6-6-6-6M8 6l-6 6 6 6"></path>
          </svg>
          <span class="card-title">PARSED JSON</span>
          <span class="card-meta">{parsed.matchedCount} records</span>
        </div>
        <button class="btn-download" on:click={downloadJSON}>
          <svg width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.2">
            <path d="M21 15v4a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2v-4"></path>
            <polyline points="7 10 12 15 17 10"></polyline>
            <line x1="12" y1="15" x2="12" y2="3"></line>
          </svg>
          Download JSON
        </button>
      </div>

      <div class="json-container">
        <div class="gutter">
          {#each Array(Math.max(parsed.results.length * 9, 20)) as _, idx}
            <div class="line-num">{idx + 1}</div>
          {/each}
        </div>
        <div class="json-body">
          {#if parsed.results.length === 0}
            <div class="json-empty">No matching records extracted yet.</div>
          {:else}
            <div class="json-bracket">[</div>
            {#each parsed.results as item, idx}
              <div class="json-entry">
                <span class="json-brace">&#123;</span>
                <div class="json-fields">
                  {#each Object.entries(item) as [k, v], fieldIdx}
                    <div class="json-field-line">
                      <span class="json-key">"{k}"</span><span class="json-colon">: </span><span class="json-val">"{v}"</span>{fieldIdx < Object.entries(item).length - 1 ? ',' : ''}
                    </div>
                  {/each}
                </div>
                <span class="json-brace">&#125;</span>{idx < parsed.results.length - 1 ? ',' : ''}
              </div>
            {/each}
            <div class="json-bracket">]</div>
          {/if}
        </div>
      </div>
    </section>
  </main>

  <!-- Bottom Panel: FIELD MAPPING -->
  <section class="mapping-card">
    <div class="mapping-header">
      <div class="mapping-title-group">
        <svg class="pane-icon blue" width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5">
          <polyline points="16 18 22 12 16 6"></polyline>
          <polyline points="8 6 2 12 8 18"></polyline>
        </svg>
        <span class="card-title">FIELD MAPPING</span>
        <span class="mapping-subtitle">(Named groups become JSON keys)</span>
      </div>

      <!-- Toggle Switch for Generated Regex -->
      <div class="toggle-group">
        <span class="toggle-label">Show generated regex</span>
        <label class="switch">
          <input type="checkbox" bind:checked={showGeneratedRegex} />
          <span class="slider"></span>
        </label>
      </div>
    </div>

    <div class="mapping-body" class:has-regex-panel={showGeneratedRegex}>
      <!-- Left: Field Rows list -->
      <div class="fields-list">
        {#each fields as field, index}
          <div class="field-row">
            <!-- Drag dots icon -->
            <div class="drag-handle" title="Field Item">
              <svg width="12" height="14" viewBox="0 0 10 16" fill="currentColor">
                <circle cx="2" cy="2" r="1.5" />
                <circle cx="8" cy="2" r="1.5" />
                <circle cx="2" cy="8" r="1.5" />
                <circle cx="8" cy="8" r="1.5" />
                <circle cx="2" cy="14" r="1.5" />
                <circle cx="8" cy="14" r="1.5" />
              </svg>
            </div>

            <!-- Group Name input -->
            <input
              type="text"
              class="input-group-name"
              bind:value={field.name}
              placeholder="field_name"
              spellcheck="false"
            />

            <!-- Pattern input -->
            <input
              type="text"
              class="input-pattern"
              bind:value={field.pattern}
              placeholder="pattern"
              spellcheck="false"
            />

            <!-- Preview sample value -->
            <div class="field-sample-preview">
              {#if parsed.fieldFirstMatch && parsed.fieldFirstMatch[cleanGroupName(field.name)]}
                <span class="sample-text">e.g. {parsed.fieldFirstMatch[cleanGroupName(field.name)]}</span>
              {:else if field.name === 'ip'}
                <span class="sample-text">e.g. 192.168.1.10</span>
              {:else if field.name === 'timestamp'}
                <span class="sample-text">e.g. 29/Sep/2026:10:21:34 +0530</span>
              {:else if field.name === 'method'}
                <span class="sample-text">e.g. GET</span>
              {:else if field.name === 'path'}
                <span class="sample-text">e.g. /login</span>
              {/if}
            </div>

            <!-- Remove Button -->
            <button
              class="btn-delete"
              title="Remove Field"
              on:click={() => removeField(index)}
            >
              ✕
            </button>
          </div>
        {/each}

        <!-- Add Field Button -->
        <div class="add-field-row">
          <button class="btn-add-field" on:click={addField}>
            <span class="plus-icon">+</span> Add Field
          </button>
        </div>
      </div>

      <!-- Right: Generated Regex Panel -->
      {#if showGeneratedRegex}
        <div class="regex-preview-panel">
          <div class="regex-panel-header">
            <span class="regex-panel-title">GENERATED REGEX</span>
            <button class="btn-copy" on:click={copyRegex}>
              <svg width="13" height="13" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                <rect x="9" y="9" width="13" height="13" rx="2" ry="2"></rect>
                <path d="M5 15H4a2 2 0 0 1-2-2V4a2 2 0 0 1 2-2h9a2 2 0 0 1 2 2v1"></path>
              </svg>
              {copied ? 'Copied!' : 'Copy'}
            </button>
          </div>

          <div class="regex-highlight-view">
            <code>
              {#each fields.filter(f => f.name && f.pattern) as f, idx}
                {#if idx > 0}<span class="rx-sep">.*?</span>{/if}
                <span class="rx-bracket">(?&lt;</span><span class="rx-name">{cleanGroupName(f.name)}</span><span class="rx-bracket">&gt;</span><span class="rx-pat">{f.pattern}</span><span class="rx-bracket">)</span>
              {/each}
            </code>
          </div>
        </div>
      {/if}
    </div>
  </section>

  <!-- Bottom Strip: CHALLENGES -->
  <footer class="challenges-bar">
    <div class="challenges-left">
      <div class="flag-icon-group">
        <svg width="14" height="14" viewBox="0 0 24 24" fill="#eab308" stroke="#eab308" stroke-width="2">
          <path d="M4 15s1-1 4-1 5 2 8 2 4-1 4-1V3s-1 1-4 1-5-2-8-2-4 1-4 1z"></path>
          <line x1="4" y1="22" x2="4" y2="15"></line>
        </svg>
        <span class="challenges-title">CHALLENGES</span>
      </div>

      <div class="challenge-pills">
        {#each CHALLENGES as ch}
          <button
            class="challenge-btn"
            class:active={activeChallenge === ch.id}
            on:click={() => selectChallenge(ch.id)}
          >
            {ch.title}
          </button>
        {/each}
      </div>
    </div>

    <div class="teaching-tip">
      <span class="lightbulb">💡</span>
      <span class="tip-text">Tip: Use named groups — <code>(?&lt;name&gt;pattern)</code></span>
    </div>
  </footer>
</div>

<style>
  .app-root {
    display: flex;
    flex-direction: column;
    min-height: 100vh;
    padding: 20px 28px 24px;
    gap: 16px;
    background-color: var(--bg-page);
  }

  /* Header */
  .header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding-bottom: 4px;
    flex-wrap: wrap;
    gap: 16px;
  }

  .logo-row {
    display: flex;
    align-items: center;
    gap: 10px;
  }

  .logo-title {
    font-size: 19px;
    font-weight: 700;
    letter-spacing: 0.05em;
    color: var(--text-main);
  }

  .badge-regex-lab {
    font-size: 10.5px;
    font-weight: 600;
    background: rgba(37, 99, 235, 0.2);
    border: 1px solid rgba(59, 130, 246, 0.4);
    color: var(--accent-cyan);
    padding: 2px 7px;
    border-radius: 4px;
    letter-spacing: 0.06em;
  }

  .logo-subtitle {
    font-size: 12px;
    color: var(--text-muted);
    margin-top: 3px;
  }

  /* Pipeline Stepper */
  .pipeline-stepper {
    display: flex;
    align-items: center;
    gap: 8px;
    background: #0f1420;
    border: 1px solid var(--border);
    padding: 5px 12px;
    border-radius: 8px;
  }

  .step-pill {
    background: #151d2e;
    border: 1px solid #243048;
    color: #cbd5e1;
    font-size: 11px;
    font-weight: 500;
    padding: 3px 9px;
    border-radius: 5px;
    white-space: nowrap;
  }

  .step-arrow {
    color: var(--text-dim);
    font-size: 11px;
  }

  /* Controls Bar */
  .controls-bar {
    display: flex;
    justify-content: space-between;
    align-items: center;
    gap: 16px;
    flex-wrap: wrap;
    padding-bottom: 2px;
  }

  .controls-left {
    display: flex;
    align-items: center;
    gap: 10px;
    flex-wrap: wrap;
  }

  .label-heading {
    font-size: 10.5px;
    font-weight: 700;
    color: var(--text-dim);
    letter-spacing: 0.08em;
  }

  .select-wrapper {
    position: relative;
    display: inline-flex;
    align-items: center;
  }

  .source-select {
    appearance: none;
    background: #111726;
    border: 1px solid var(--border);
    color: #e2e8f0;
    padding: 7px 32px 7px 12px;
    font-size: 12.5px;
    border-radius: 7px;
    outline: none;
    cursor: pointer;
    transition: border-color 0.15s ease;
  }

  .source-select:hover {
    border-color: var(--border-light);
  }

  .source-select:focus {
    border-color: var(--accent-blue);
  }

  .select-caret {
    position: absolute;
    right: 10px;
    color: var(--text-muted);
    pointer-events: none;
    font-size: 11px;
  }

  .hidden-file-input {
    display: none;
  }

  .btn-upload {
    display: inline-flex;
    align-items: center;
    gap: 6px;
    background: #111726;
    border: 1px solid var(--border);
    color: #cbd5e1;
    padding: 7px 14px;
    font-size: 12px;
    font-weight: 500;
    border-radius: 7px;
    cursor: pointer;
    transition: all 0.15s ease;
  }

  .btn-upload:hover {
    background: #172033;
    border-color: var(--border-light);
    color: #ffffff;
  }

  .upload-icon {
    font-size: 13px;
    color: var(--text-muted);
  }

  /* Panes Grid */
  .panes-grid {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 16px;
  }

  .card-pane {
    background: var(--bg-card);
    border: 1px solid var(--border);
    border-radius: 10px;
    display: flex;
    flex-direction: column;
    overflow: hidden;
    height: 380px;
  }

  .card-header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 10px 16px;
    background: #0d121c;
    border-bottom: 1px solid var(--border);
  }

  .card-title-group {
    display: flex;
    align-items: center;
    gap: 8px;
  }

  .pane-icon {
    color: var(--text-muted);
  }

  .pane-icon.blue {
    color: #60a5fa;
  }

  .card-title {
    font-size: 11px;
    font-weight: 700;
    letter-spacing: 0.08em;
    color: var(--text-main);
  }

  .card-meta {
    font-size: 11px;
    color: var(--text-dim);
    font-family: var(--font-mono);
  }

  .btn-download {
    display: inline-flex;
    align-items: center;
    gap: 6px;
    background: #2563eb;
    border: 1px solid #3b82f6;
    color: #ffffff;
    padding: 5px 12px;
    font-size: 11.5px;
    font-weight: 600;
    border-radius: 6px;
    cursor: pointer;
    transition: background 0.15s ease;
  }

  .btn-download:hover {
    background: #1d4ed8;
  }

  /* Code Editors with Gutter */
  .json-container {
    display: flex;
    flex: 1;
    background: var(--bg-inner);
    overflow: auto;
    font-family: var(--font-mono);
    font-size: 12px;
    line-height: 1.6;
  }

  .gutter {
    padding: 12px 10px;
    background: #080b12;
    border-right: 1px solid #141a29;
    user-select: none;
    text-align: right;
    min-width: 38px;
  }

  .line-num {
    color: #3b465e;
    font-size: 11px;
  }

  .log-entries-scroll {
    flex: 1;
    background: var(--bg-inner);
    overflow-y: auto;
    overflow-x: hidden;
    font-family: var(--font-mono);
    font-size: 12px;
    line-height: 1.6;
    padding: 10px 0;
  }

  .log-line-row {
    display: flex;
    align-items: flex-start;
    padding: 1px 0;
    transition: background 0.1s ease;
  }

  .log-line-row:hover {
    background: rgba(255, 255, 255, 0.02);
  }

  .log-line-num {
    width: 44px;
    padding: 0 10px 0 6px;
    color: #3b465e;
    font-size: 11px;
    text-align: right;
    user-select: none;
    flex-shrink: 0;
    line-height: 1.6;
    border-right: 1px solid #141a29;
  }

  .log-line-content {
    flex: 1;
    padding: 0 14px;
    color: #94a3b8;
    white-space: pre-wrap;
    word-break: break-all;
    line-height: 1.6;
    outline: none;
    min-width: 0;
  }

  .log-line-content:focus {
    color: #f1f5f9;
  }

  .json-body {
    flex: 1;
    padding: 12px 14px;
    color: #cbd5e1;
    overflow-x: auto;
  }

  .json-empty {
    color: var(--text-dim);
    font-style: italic;
    padding: 10px 0;
  }

  .json-bracket {
    color: #e2e8f0;
  }

  .json-entry {
    margin-left: 12px;
    margin-bottom: 4px;
  }

  .json-brace {
    color: #94a3b8;
  }

  .json-fields {
    margin-left: 16px;
  }

  .json-field-line {
    white-space: nowrap;
  }

  .json-key {
    color: #60a5fa;
  }

  .json-colon {
    color: #cbd5e1;
  }

  .json-val {
    color: #4ade80;
  }

  /* Mapping Card */
  .mapping-card {
    background: var(--bg-card);
    border: 1px solid var(--border);
    border-radius: 10px;
    padding: 14px 18px;
    display: flex;
    flex-direction: column;
    gap: 12px;
  }

  .mapping-header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    border-bottom: 1px solid #141a29;
    padding-bottom: 10px;
  }

  .mapping-title-group {
    display: flex;
    align-items: center;
    gap: 8px;
  }

  .mapping-subtitle {
    font-size: 11px;
    color: var(--text-dim);
  }

  /* Switch Toggle */
  .toggle-group {
    display: flex;
    align-items: center;
    gap: 10px;
  }

  .toggle-label {
    font-size: 11px;
    color: var(--text-muted);
  }

  .switch {
    position: relative;
    display: inline-block;
    width: 34px;
    height: 18px;
  }

  .switch input {
    opacity: 0;
    width: 0;
    height: 0;
  }

  .slider {
    position: absolute;
    cursor: pointer;
    top: 0; left: 0; right: 0; bottom: 0;
    background-color: #1e293b;
    transition: .2s;
    border-radius: 18px;
    border: 1px solid #334155;
  }

  .slider:before {
    position: absolute;
    content: "";
    height: 12px;
    width: 12px;
    left: 2px;
    bottom: 2px;
    background-color: white;
    transition: .2s;
    border-radius: 50%;
  }

  input:checked + .slider {
    background-color: #2563eb;
    border-color: #3b82f6;
  }

  input:checked + .slider:before {
    transform: translateX(16px);
  }

  /* Mapping Body */
  .mapping-body {
    display: flex;
    gap: 20px;
  }

  .fields-list {
    flex: 1;
    display: flex;
    flex-direction: column;
    gap: 8px;
    min-width: 0;
  }

  .field-row {
    display: flex;
    align-items: center;
    gap: 8px;
    background: #090d15;
    border: 1px solid #192233;
    padding: 6px 10px;
    border-radius: 6px;
  }

  .drag-handle {
    color: #3b465e;
    cursor: grab;
    display: flex;
    align-items: center;
  }

  .input-group-name {
    width: 130px;
    background: #06090f;
    border: 1px solid #202b3d;
    color: #60a5fa;
    padding: 5px 10px;
    font-family: var(--font-mono);
    font-size: 11.5px;
    border-radius: 5px;
    outline: none;
  }

  .input-group-name:focus {
    border-color: #3b82f6;
  }

  .input-pattern {
    flex: 1;
    background: #06090f;
    border: 1px solid #202b3d;
    color: #7dd3fc;
    padding: 5px 10px;
    font-family: var(--font-mono);
    font-size: 11.5px;
    border-radius: 5px;
    outline: none;
    min-width: 140px;
  }

  .input-pattern:focus {
    border-color: #3b82f6;
  }

  .field-sample-preview {
    width: 170px;
    overflow: hidden;
    text-overflow: ellipsis;
    white-space: nowrap;
    text-align: right;
  }

  .sample-text {
    font-family: var(--font-mono);
    font-size: 10.5px;
    color: #475569;
  }

  .btn-delete {
    background: transparent;
    border: none;
    color: #475569;
    font-size: 12px;
    cursor: pointer;
    padding: 2px 6px;
    border-radius: 3px;
    transition: all 0.15s ease;
  }

  .btn-delete:hover {
    color: #ef4444;
    background: rgba(239, 68, 68, 0.1);
  }

  .add-field-row {
    margin-top: 4px;
  }

  .btn-add-field {
    display: inline-flex;
    align-items: center;
    gap: 6px;
    background: #111827;
    border: 1px solid #243048;
    color: #93c5fd;
    padding: 6px 14px;
    font-size: 12px;
    font-weight: 500;
    border-radius: 6px;
    cursor: pointer;
    transition: all 0.15s ease;
  }

  .btn-add-field:hover {
    background: #1e293b;
    border-color: #3b82f6;
    color: #ffffff;
  }

  .plus-icon {
    font-size: 13px;
  }

  /* Right Regex Preview Panel */
  .regex-preview-panel {
    width: 380px;
    background: #070a10;
    border: 1px solid #1a2233;
    border-radius: 8px;
    padding: 12px 14px;
    display: flex;
    flex-direction: column;
    gap: 8px;
  }

  .regex-panel-header {
    display: flex;
    justify-content: space-between;
    align-items: center;
  }

  .regex-panel-title {
    font-size: 10.5px;
    font-weight: 700;
    color: var(--text-dim);
    letter-spacing: 0.08em;
  }

  .btn-copy {
    display: inline-flex;
    align-items: center;
    gap: 5px;
    background: transparent;
    border: none;
    color: var(--text-muted);
    font-size: 11px;
    cursor: pointer;
    padding: 2px 6px;
    border-radius: 4px;
    transition: color 0.15s ease;
  }

  .btn-copy:hover {
    color: #ffffff;
  }

  .regex-highlight-view {
    font-family: var(--font-mono);
    font-size: 11.5px;
    line-height: 1.6;
    word-break: break-all;
    overflow-y: auto;
    max-height: 180px;
    color: #94a3b8;
  }

  .rx-bracket {
    color: #eab308;
  }

  .rx-name {
    color: #f97316;
    font-weight: 600;
  }

  .rx-pat {
    color: #38bdf8;
  }

  .rx-sep {
    color: #64748b;
  }

  /* Challenges Bar */
  .challenges-bar {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding-top: 4px;
    flex-wrap: wrap;
    gap: 12px;
  }

  .challenges-left {
    display: flex;
    align-items: center;
    gap: 12px;
    flex-wrap: wrap;
  }

  .flag-icon-group {
    display: flex;
    align-items: center;
    gap: 6px;
  }

  .challenges-title {
    font-size: 10.5px;
    font-weight: 700;
    color: var(--text-dim);
    letter-spacing: 0.08em;
  }

  .challenge-pills {
    display: flex;
    gap: 8px;
    flex-wrap: wrap;
  }

  .challenge-btn {
    background: #0f1420;
    border: 1px solid var(--border);
    color: var(--text-muted);
    font-size: 11.5px;
    padding: 5px 12px;
    border-radius: 6px;
    cursor: pointer;
    transition: all 0.15s ease;
    white-space: nowrap;
  }

  .challenge-btn:hover {
    color: #ffffff;
    border-color: #334155;
  }

  .challenge-btn.active {
    background: #1e3a8a;
    border-color: #3b82f6;
    color: #60a5fa;
    font-weight: 600;
  }

  .teaching-tip {
    display: flex;
    align-items: center;
    gap: 6px;
    font-size: 11.5px;
    color: var(--text-dim);
  }

  .tip-text code {
    font-family: var(--font-mono);
    color: #94a3b8;
    background: #0f1420;
    padding: 2px 6px;
    border-radius: 4px;
    border: 1px solid var(--border);
  }

  @media (max-width: 950px) {
    .panes-grid {
      grid-template-columns: 1fr;
    }
    .mapping-body {
      flex-direction: column;
    }
    .regex-preview-panel {
      width: 100%;
    }
    .field-sample-preview {
      display: none;
    }
  }
</style>
