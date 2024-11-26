# Downtime
Extremely simple, static, server-free uptime monitoring. No procurement or ATO needed*.

\*Statements about ATO not yet verified.

[View the demo site.](https://18f.github.io/downtime/)

## What problem does this solve?

This monitors the uptime/downtime status of a single website, without:

- needing an [ATO](https://digital.gov/resources/an-introduction-to-ato/)
- procuring and setting up a commercial monitoring solution
- hosting a server
- running a database


## How can I use this?

- Clone or [fork](https://github.com/18F/downtime/fork) this repository to your own GitHub account / organization.
- Delete the contents of `log.txt` and `readlog.txt`, but leave the empty files in place.
- Find and replace `18f.gsa.gov` everywhere in the repo with your own URL. As of this writing, it's only in:
  - The header in `index.html`
  - The `curl` line in `.github/workflows/ping.yaml`
- Make sure GitHub Pages is enabled to deploy your `main` branch at `/`.

The page will update a little while after every status check, which by default runs every 15 minutes.

To run the server check manually, to make sure it's working or to force the first deploy:

- Go to the Actions tab.
- In the left sidebar, select "Check server status".
- In the light blue bar, there will be a "Run workflow" dropdown.
- Click it, make sure the branch is "main", and click "Run workflow".


## How does it work?

This monitors a single site and records the uptime/downtime in a log. It displays the last 65 events, showing the uptime/downtime in a simple chart.

Process:

- Every fifteen minutes, a scheduled GitHub Action checks the status of the website.
- It records the status in a log, `log.txt`. The log is append-only, so we can track uptime over all time.
- It copies the last 65 events to a separate log, `readlog.txt`. Instead of reading a log with potentially thousands of entries, we only load the smaller set of entries in `readlog.txt` for display.
- The scheduled GitHub action commits these changes, saving the data, using these files as the data source.
- Each commit, GitHub redeploys the GitHub Pages page with the new data.


## The next few tasks

- Check for accessibility / 508 compliance
- Multi-site monitoring
- Settings file for monitored domains, cadence, etc.

