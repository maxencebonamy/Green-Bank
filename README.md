# Green Bank

A loan rate calculator for buying a vehicle, based on its environmental impact.

The user describes a vehicle (type, energy, yearly distance, year of manufacture, number of passengers) and gets a loan rate. The cleaner the vehicle, the lower the rate. The project was built for the Asynconf 2023 tournament, where it won first place, and is deployed at [green-bank.maxencebonamy.fr](https://green-bank.maxencebonamy.fr).

## Table of contents

- [Green Bank](#green-bank)
	- [Table of contents](#table-of-contents)
	- [How it works](#how-it-works)
	- [Tech stack](#tech-stack)
	- [Repository structure](#repository-structure)
	- [Prerequisites](#prerequisites)
	- [Installation](#installation)
	- [Running locally](#running-locally)
	- [Useful scripts](#useful-scripts)
	- [Tests and quality](#tests-and-quality)
	- [Deployment](#deployment)
	- [License](#license)

## How it works

1. **Inputs**: the page is split into sections. The user picks a vehicle type, an energy and a number of passengers on cards, and sets the yearly distance and the year of manufacture with sliders. Sliders start at the middle value of their data file.
2. **Score**: each of the vehicle type, energy, distance and year options carries an ecological mark out of 10. The score is the sum of the four marks, rounded to the nearest integer (a decimal part of exactly 0.5 rounds down).
3. **Rate**: the rounded score is matched against the score ranges of `loan_rates.json` to get a base rate. The passenger option then adds or subtracts its `extra` value.
4. **Result**: the final rate is computed again every time an input changes, and is displayed once all inputs are set. The page can be reset.
5. **Data-driven options**: options, marks and rate ranges are read from JSON files in `src/assets/data`. Editing a file changes the site without touching the components.

The site is responsive and has a dark and a light theme, dark by default.

## Tech stack

| Layer | Technologies |
| --- | --- |
| Frontend | Next.js 14 (App Router), React 18, Tailwind CSS 3, Radix UI, shadcn/ui, `class-variance-authority`, `next-themes`, Lucide, `react-parallax-tilt` |
| Language | TypeScript 5 |
| Data | Static JSON files |
| Tooling | npm, Jest, ts-jest, ESLint (`@bluzzi/eslint-config`) |
| Deployment | Replit, Vercel |

## Repository structure

```
.
├── src/
│   ├── app/             Next.js entry: layout, main page, global styles
│   ├── assets/
│   │   ├── data/        JSON data files (vehicles, energies, distances, years, passengers, loan rates)
│   │   └── images/      Images and icons
│   ├── components/
│   │   ├── navbar/      Navigation bar and theme toggle
│   │   ├── sections/    One folder per page section (home, car, energy, distance, year, passengers, result)
│   │   └── ui/          shadcn/ui components
│   └── lib/             Rate algorithm, types, helpers
└── tests/               Jest tests, same tree as src
```

Types in `src/lib/types.ts` are inferred from the JSON files. The `@/*` alias points to `src/*`. Components in `src/components/ui` are excluded from linting. Each data file is documented in [`src/assets/data/README.md`](src/assets/data/README.md), which lists the constraints to respect when editing them (unique ids, contiguous ranges).

## Prerequisites

- [Node.js](https://nodejs.org/) 18 (the version declared in `replit.nix`)
- npm

## Installation

```bash
git clone git@github.com:maxencebonamy/Green-Bank.git
cd Green-Bank
npm install
```

## Running locally

```bash
npm run dev
```

The application is available at <http://localhost:3000>. No environment variable is needed.

## Useful scripts

| Command | Effect |
| --- | --- |
| `npm run dev` | Start the development server |
| `npm run build` | Build for production |
| `npm run start` | Serve the production build |
| `npm run lint` | Lint the repository |
| `npm run lint:fix` | Lint and auto-fix |
| `npx jest` | Run the tests |

## Tests and quality

```bash
npx jest
npm run lint
```

Tests use Jest with ts-jest and live in `tests/`. They cover the rate algorithm (including the cases given in the tournament statement) and the formatting helpers. Components are not tested.

## Deployment

The repository has no CI. [`.replit`](.replit) defines a Replit deployment (Cloud Run target) that runs `npm run build`, then `npm run start`. The production site is hosted on Vercel.

## License

This project is licensed under the [MIT License](LICENSE).
