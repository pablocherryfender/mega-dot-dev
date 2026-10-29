# S01E03 · what it shows

The support desk merged 412 reports about dates into one ticket, #4471, and marked it known.
Read by tenant, booking form and what people describe, it holds three problems:

| Problem | Reports | Ticket |
|---|---|---|
| `parser`: day-first dates read month first | 374 | [#1](https://github.com/pablocherryfender/mega-dot-dev/issues/1) |
| `hint`: booking-v3 form hint says `MM/DD/YYYY` | 33 | [#2](https://github.com/pablocherryfender/mega-dot-dev/issues/2) |
| `one-day`: dates move by one day, no swap | 5 | [#3](https://github.com/pablocherryfender/mega-dot-dev/issues/3) |

## Files

- `support-kb/`: the desk's kb export as it came, with one page added:
  [`src/routes/kb/incidents/4471/split/+page.svx`](support-kb/src/routes/kb/incidents/4471/split/+page.svx). It uses only the desk's own components.
- [`rendered/kb-incidents-4471-split.html`](rendered/kb-incidents-4471-split.html): the page as the desk sees it (SvelteKit + mdsvex, prerendered)
