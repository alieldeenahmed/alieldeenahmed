## Ali El Deen Ahmed

**Junior Backend Engineer · C# / ASP.NET Core / PostgreSQL**

I build backend systems in C# and ASP.NET Core on PostgreSQL. I spend most of the effort on the parts that fail in production: concurrent writes, authorization boundaries, and unreliable dependencies. Where I can, I make the database enforce the rules, and I test that with concurrent requests against a real PostgreSQL server. I'm a first-year Business student at Egypt-Japan University of Science and Technology (EJUST), and I built each project below on my own.

### What I work with

- **Backend:** C#, ASP.NET Core, EF Core, REST APIs; Node.js with Fastify when a project needs it
- **Database:** PostgreSQL, including constraints, indexes, row and advisory locks, and migrations
- **Frontend:** TypeScript, React, and Next.js, enough to ship a complete product
- **Quality:** unit, integration, and concurrency tests, with CI on GitHub Actions

### Featured projects

#### [CEMS](https://github.com/alieldeenahmed/CEMS): management system for a multi-branch education center
`ASP.NET Core` `EF Core` `PostgreSQL` `MediatR` `React` `TypeScript`

It handles branches, teachers, students, scheduling, billing, and payroll. The Owner sees every branch, managers and front-desk staff see only their own branch, and teachers see only what they teach.

- Clean Architecture with 114 MediatR command/query handlers. 71 of them check branch scope, and the rest are Owner-only, own-data or identity operations behind a role check. On every request the API re-reads the account's status, roles, and branches from the database, so it doesn't rely on what the JWT claimed at login.
- Two layers stop double-booking: advisory locks taken in a deterministic order before each conflict check, and PostgreSQL `EXCLUDE USING gist` constraints behind them. A partial unique index allows only one live enrollment per student per course.
- 527 backend tests. 44 of them run against real PostgreSQL, and 18 of those release requests at the same instant to prove that race conditions are actually blocked. The React frontend has another 264 tests.

#### [Noon-Scraper](https://github.com/alieldeenahmed/Noon-Scraper): price tracking and discount analysis for noon.com
`ASP.NET Core` `PostgreSQL` `Playwright` `GitHub Actions` · [live demo](https://noon-scraper-phi.vercel.app)

It crawls product listings every day, stores each reading as price history, and flags discounts that don't hold up against a product's own past prices.

- The API has no browser, so GitHub Actions jobs do all the scraping. Actions can deliver a job twice or not at all, so the database is the source of truth. Jobs move through a state machine (`Pending → Running → Completed | Failed`). A worker claims a job with a single conditional `UPDATE`, and each price reading is recorded in one transaction behind a per-product advisory lock.
- Submitted URLs are checked against an allow-list, with hosts compared in punycode form, so look-alike domains are rejected and the crawler's browser can't be pointed at another site. Endpoints that consume Actions minutes are limited by rate limits and by budgets stored in the database.
- 699 backend tests: 360 use a real PostgreSQL, and 13 are explicit concurrency tests that each assert exactly one party wins. CI runs them on pushes to `main` and on pull requests.

#### [Freyya](https://github.com/alieldeenahmed/Freyya): skincare storefront with an order API and admin dashboard
`Next.js` `TypeScript` `Fastify` `PostgreSQL` `Drizzle` · [live demo (full stack)](https://freyya.vercel.app/), a public sandbox that resets nightly

The storefront runs on a Fastify API that prices orders on the server, can't oversell stock, and handles retried requests safely.

- Checkout locks stock rows in a fixed order (`SELECT … FOR UPDATE`) and holds stock for 15 minutes. An `Idempotency-Key` sent from the browser makes a retried checkout create only one order. Concurrency tests on real PostgreSQL cover cases such as ten buyers for three units, where exactly three orders succeed, and the same race gave the same result against the deployed API.
- **Limitations:** payment is simulated.

### Achievements

- 256+ problems solved on Codeforces
- ECPC Teens: 68th in 2024, 60th in 2025
- Egyptian Olympiad in Informatics participant
- NASA Space Apps Challenge 2024 participant
- STEM Obour CS Club: team leader of a 5-person team and mentor. One frontend trainee I mentored later became a tutor.

### Links

- LinkedIn: https://www.linkedin.com/in/alieldeen
- Codeforces: https://codeforces.com/profile/ali.eldeen008
- Email: alieldeen.ah@gmail.com