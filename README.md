# Easy Payroll: Multi-tenant Payroll for Philippine Companies

**A payroll system that gets night shifts, holidays and overtime right, and hands the result to your books.**
It computes SSS, PhilHealth, Pag-IBIG and withholding tax, handles day, mid, night and rotating
shifts (including shifts that cross midnight), gives employees their own sign-in for time logs,
leave and payslips, and passes every finalized run to **Easy Aiccounting**, its companion accounting
app. Built end to end as a single .NET codebase. (DevPAYROLL is the code name; the product is called
Easy Payroll.) Current version: 0.37.1.

It runs three ways: stand-alone on one PC, an office server on the client's local network, or
hosted online.

> The source code is private because this is a commercial product. I'm happy to give reviewers
> read access or walk through the code on a call, so feel free to reach out.

![Easy Payroll](screenshots/banner.png)

> Every name and amount in these images is made-up sample data. The peso figures are examples,
> not official government rates.

---

## Why I built it

Payroll in the Philippines is full of rules that are easy to get slightly wrong: night
differential only on the hours inside the night window, premiums that multiply instead of add,
holiday and rest-day rates that stack with overtime, contributions taken once a month, and a net
pay that must never go negative. Easy Payroll is built around those rules, and it is meant to be
licensed to many companies, so every company's data is kept separate on the server.

## Main features

- **Shifts that cross midnight are just shifts.** Day, mid, night and rotating schedules; a night
  shift belongs to the day it started, so pay and attendance line up.
- **Premiums multiply, they don't add.** Night overtime at 150 an hour is 150 × 1.25 × 1.10 = 206.25,
  not 202.50. Holiday and rest-day rates stack with overtime and night differential.
- **Net pay never below zero.** Contributions first, then loans cut to what fits, with the rest kept on
  the balance and the line flagged for review.
- **Rates are data, not a release.** Versioned statutory tables, updated through a digitally signed
  rate pack that is reviewed before it is applied.
- **Time clock at the door.** Staff type their employee number, the server decides in or out, and door
  PCs keep taking taps while the network is down.
- **Employee self-service.** Own sign-in, schedule, leave, corrections and payslips.
- **A finished run goes to the books.** Each finalized run arrives in Easy Aiccounting as drafts for an
  accountant to review and post (see below).
- **One product, three ways to run it.** Stand-alone, office server or online, with signed updates.

## Connects to Easy Aiccounting

Easy Aiccounting is the companion app for the same company: a chart of accounts, journal entries,
a general ledger, a trial balance and payables, bills and supplier payments, remittances to the agencies,
printed checks, petty cash and basic financial statements. It is a separate product, licensed separately.

![Accounting link](screenshots/accounting-link.png)

- **Pull, not push.** Easy Aiccounting asks Easy Payroll for finalized runs it has not seen yet, and each
  run is imported once.
- **Read-only link key.** An admin issues a key in Settings and can revoke it. The key can read finalized
  runs and nothing else, and only the key's hash is stored.
- **Totals, not people.** The feed carries each run's totals (gross, deductions, loan repayments, net
  pay), never employee names.
- **Checked before it is booked.** Gross pay and tax refund less deductions must equal net pay, and loan
  repayments must equal the other deductions. A run that fails is refused with the difference shown.
- **Two drafts per run.** An accrual entry and a payment entry, left as drafts for an accountant to review
  and post. A posted entry is never edited; a mistake is fixed with a reversal.
- **Every centavo exact.** Amounts keep 4 decimal places, and nothing is rounded silently.

Easy Aiccounting records and prints. It sends nothing to SSS, PhilHealth, Pag-IBIG or BIR and files no
return, and its reports say "Not a BIR-registered book of accounts."

## Screenshots

**Dashboard**: today's figures first, then a numbered list of what needs you and the next step of the run.

![Dashboard](screenshots/dashboard.png)

**Settings home**: each area shows its state at a glance, with a highlight that follows the row you point at.

![Settings](screenshots/settings-home.png)

**Payroll run**: compute, review every line with its contributions and tax, then finalize. The rates here are made-up sample tables.

![Payroll run](screenshots/payroll-run.png)

**Door time clock**: staff type their employee number, and the server decides in or out.

![Door time clock](screenshots/door-time-clock.png)

**Clocked in**: the confirmation, with a few seconds to undo a wrong tap.

![Clocked in](screenshots/door-time-clock-punched.png)

**Premiums multiply, they don't add**: night overtime at 150 an hour is 150 × 1.25 × 1.10 = 206.25, not 150 × 1.35 = 202.50.

![Premiums multiply](screenshots/premiums-math.png)

**Premium pay on the payslip**: hours, rate and amount for every premium, plus the rest-day and holiday rates.

![Premium pay](screenshots/premiums.png)

**Net pay never below zero**: contributions first, then loans cut to what fits, with the rest kept on the balance and the line flagged for review.

![No negative pay](screenshots/no-negative-pay.png)

**Fixed salary stays fixed**: a short cutoff still pays the full period amount; absences and lateness are deducted.

![Fixed salary](screenshots/fixed-salary.png)

**You choose when contributions are taken**: SSS, PhilHealth and Pag-IBIG on the last cutoff, the first, split evenly or a specific one, per pay group.

![Contribution timing](screenshots/contribution-timing.png)

**Rates are data, not a release**: versioned tables updated through a digitally signed rate pack that is reviewed before it is applied.

![Rate packs](screenshots/rate-packs.png)

**Shifts**: day, mid, night and rotating schedules, with weekly rest days and breaks.

![Shifts](screenshots/shifts.png)

**Payslip**: hours × rate on every earning, statutory deductions (sample rates), year to date, and a company-arranged layout.

![Payslip](screenshots/payslip.png)

**Employee profile**: tabs that slide, with a mark on any tab that needs attention.

![Employee profile](screenshots/employee-edit.png)

**Employee self-service**: clock in and out (stamped by the server), announcements, schedule, leave balances and payslips.

![Self-service](screenshots/self-service.png)

**My Schedule**: every day shows how it went: on time, late, absent, rest day, holiday or leave.

![My Schedule](screenshots/my-schedule.png)

**Sign-in**: the room changes with the hour, with your company mark in the scene.

![Sign-in](screenshots/sign-in.png)

## Marketing material

Ready-to-post slides and captions (English and Taglish) are in [`marketing/`](marketing/): a banner and
nine square slides (shifts, premiums, payslips, loans, self-service, dashboard, the three ways to run it and
the accounting link), plus the poster set. All of it uses sample data.

## Features

### Time and shifts
- Shift definitions with breaks (overnight shifts included), checked so a break sits inside its shift
- Fixed or rotating weekly assignments with rest days, bulk assign, and a weekly roster with mixed schedules and Copy week
- Attendance entry, a quick-entry screen for a whole cutoff, import from CSV or Excel with a saved column mapping, and employee clock in and out stamped by the server
- A door time clock on a PC or tablet: staff type their employee number, the server decides in or out, an optional photo goes with each punch, and door PCs keep taking taps while the network is down (taps that don't fit go to HR instead of becoming time logs)
- Overtime counted per day, with optional authorization before hours past 8 are paid, and a per-employee setting for staff who are not entitled to overtime
- Proof of work for field staff: photos or PDFs added to a time log, which HR can review and download
- Company and national holidays, with per-country calendars and per-branch holidays
- Time-log corrections with a required reason, and employee correction requests staff approve
- A date that a finalized run already paid stays as paid: edits, imports and corrections to it are refused

### Pay
- Pay groups: weekly, semi-monthly or monthly, each computed on its own schedule
- Night differential (a company setting), overtime, holiday and rest-day premiums, compounded the way the rules are
- Fixed amount per period for salaried staff, or hours worked × rate, and a company setting for working days per year
- SSS, PhilHealth and Pag-IBIG taken once a month, with the timing chosen per pay group, and withholding tax by pay frequency
- Allowances per run, per day or per week (a part week pays by the day), and attendance awards: a meal allowance per completed day and a monthly perfect-attendance award
- Year-end tax settlement and BIR 2316 data, 13th month pay at the rate each period was paid, bonuses, leave encashment and year-end leave conversion
- Versioned statutory rate tables, updated through a signed rate pack, so a rate change is data and not a release
- Payslips on screen and as PDF from one model, with a layout each company arranges
- Government remittance exports (SSS, PhilHealth, Pag-IBIG, BIR) as CSV data files, not the official forms
- A finalize step that refuses when attendance, schedules or approved leave changed after compute, or while corrections and door taps are still waiting for HR

### Loans and leave
- Salary loans, company loans and cash advances, taken until paid, never below zero net pay
- Salary deductions for loss or damage, all at once or in instalments, with a required reason shown on the payslip
- Leave filed by the day, with half days, approvals, cancellation, balances, carry-over and payroll integration

### People and self-service
- Employee records with photos, positions per department, a per-field change log, org structure and a manager's My Team page
- Bank and e-wallet details kept as a protected record, visible only to roles that are given the right
- Employee requirements with renewal reminders, and custom roles and permissions per company
- Performance evaluations with probation checkpoints, incident reports and disciplinary case records
- Own sign-in for employees, announcements with pictures, PDFs and read tracking, private message threads
- Dashboard with a calendar of pay days and reminders, a quick-open search (Ctrl+K), reports in CSV and PDF, bell notifications, dark mode, appearance choices, adjustable text size, and password-encrypted backups in the stand-alone edition

### Running it
- **Stand-alone:** one Windows PC, with one-click updates from a signed notice, a backup first and a way to go back
- **Office server:** one server on the local network, other PCs pair with a code, and a status window shows connected PCs and why the server is stopped, if it is
- **Online:** hosted for you, each company's data kept separate
- Setup programs for each, an offline option that carries the database engine, and a "Start live payroll" step that clears test activity once and keeps the setup

### Licensing and updates
- Licence codes checked offline, and plans whose features decide what can be added, while payroll, payslips, reports and government files work on every plan
- A licence request sent from Settings, with the code saving itself when it is issued

## Tech stack

| Area | Technology |
|---|---|
| API | ASP.NET Core minimal APIs (.NET 10), Identity, JWT bearer with rotating refresh tokens |
| Data | EF Core 10, SQL Server (SQLite for the stand-alone edition), global query filters for tenant isolation |
| Client | .NET MAUI Blazor Hybrid (Windows), Bootstrap 5.3, custom components |
| PDF | QuestPDF |
| Signing | ECDSA P-256 signed rate packs and update notices |
| Testing | xUnit: over 2,000 automated tests across the domain, infrastructure and API projects |
| Development | Visual Studio, GitHub, AI-assisted development with Claude Code |

## How it's built

- **Pure domain layer:** entities and the shift and pay calculators have no EF or ASP.NET
  references, so they are unit-tested on their own.
- **Multi-tenant by construction:** every tenant-owned entity is filtered by company in one place,
  and a dedicated test guards the filter against cross-company leaks.
- **Integer minutes, then hours once:** time is summed in whole minutes across records and
  converted to decimal hours a single time, so totals don't drift.
- **No rounding of amounts:** rates divide last and money keeps its full precision, so a payslip
  adds up exactly.
- **One model, two renderers:** the payslip is built once and drawn by both the PDF and the
  screen, so they can't disagree.
- **Rates are data, not code:** statutory tables are versioned rows with effective dates.
- **A computed payslip is a snapshot:** it keeps the rate and settings it used, so changing a
  setting later never alters a finalized run.
- **A narrow door to the books:** the accounting feed is GET-only, takes a revocable key instead of a
  user sign-in, and serves only runs finalized at least a minute ago so none is skipped.

## About me

I'm **Mark Angelo Pasion**, a Senior Software Engineer from Malolos, Bulacan, with 10+
years of experience in C#, VB.NET, SQL Server, and SAP Business One integration.
Easy Payroll is one of my own products, designed and built end to end.

- GitHub: [@arkienz](https://github.com/arkienz)
- See also: [ArkBrew, formerly DevPOS](https://github.com/arkienz/DevPOS-Showcase)

Not affiliated with SSS, PhilHealth, Pag-IBIG (HDMF), BIR or DOLE.
