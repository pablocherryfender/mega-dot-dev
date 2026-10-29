<script lang="ts">
  import reports from '#lib/data/reports-4471.json';
  import notes from '#lib/data/desk-notes.json';
  import { splitTicket, type Problem } from '#lib/kb/problems';
  import { tenantOf } from '#lib/kb/locales';
  import type { Report } from '#lib/kb/causes';

  // issues: where each problem is tracked now, problem → link.
  let { ticket, issues = {} }: { ticket: number; issues?: Partial<Record<Problem, string>> } = $props();

  const split = $derived(splitTicket(reports as Report[], ticket, tenantOf, notes.oneDayMove));
  const channels = (c: Record<string, number>) =>
    Object.entries(c)
      .sort((a, b) => b[1] - a[1])
      .map(([ch, n]) => `${ch} ×${n}`)
      .join(' · ');
  const issueNo = (url: string) => `#${url.split('/').pop()}`;
</script>

<p class="sum">
  <strong>{split.unique}</strong> reports merged into #{ticket} ·
  <strong>{split.groups.length}</strong>
  {split.groups.length === 1 ? 'problem' : 'problems'}
</p>
<p class="note">
  The export holds {split.rows} rows for #{ticket}; {split.rows - split.unique} of them repeat a report id
  (the export's pages overlap). Each report is counted once.
</p>

<table>
  <thead>
    <tr><th>problem</th><th>reports</th><th>tenants</th><th>came in by</th><th>seen</th><th>tracked in</th></tr>
  </thead>
  <tbody>
    {#each split.groups as g (g.problem)}
      <tr class:unplaced={g.problem === 'unplaced'}>
        <td><code>{g.problem}</code></td>
        <td><strong>{g.reports}</strong></td>
        <td title={g.tenants.join(', ')}>{g.tenants.length}</td>
        <td class="ch">{channels(g.channels)}</td>
        <td>{g.firstSeen} → {g.lastSeen}</td>
        <td>
          {#if issues[g.problem]}<a href={issues[g.problem]}>{issueNo(issues[g.problem] ?? '')}</a>{:else}—{/if}
        </td>
      </tr>
    {/each}
  </tbody>
</table>

<style>
  .sum { font-size: 1.1rem; }
  .note, .ch { color: var(--muted); font-size: 0.85em; }
  .unplaced td { background: var(--warn-bg); }
</style>
