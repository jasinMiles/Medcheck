# MedCheck Frontend

MedCheck is a medical care app that helps families get trusted care for
elderly or sick loved ones.

## The problem

Families need a trusted way to refer elderly or sick loved ones to vetted
doctors, nurses and pharmacies for physical or mental health care.

## Who it is for

- Families and caregivers arranging care for someone else
- Patients, including elderly and sick people
- Healthcare providers: doctors, nurses and pharmacies

## Tech stack

- React (JavaScript) with Vite
- Tailwind CSS
- Zod for form and data validation
- ESLint for linting

## Getting started

1. Clone the repo
2. Run `npm install`
3. Copy `.env.example` to `.env` and fill in the values
4. Run `npm run dev`
5. Open the link shown in the terminal (usually http://localhost:5173)

## Project structure

- `src/components`: reusable UI pieces
- `src/pages`: one folder per screen
- `src/services`: API and video SDK code
- `src/hooks`: custom React hooks
- `src/context`: shared app state
- `src/routes`: page routing
- `src/utils`: helper functions
- `src/styles`: global styles
- `docs`: sprint documents

## Git workflow

- Never push directly to `main`
- Create a branch for each task: `setup/...`, `feature/...`, `fix/...`
- Commit small and often, with clear messages
- Open a pull request and ask a teammate to review before merging

## Environment variables

Never commit real keys. Put them in `.env`, which is ignored by Git.

## Team

- Frontend lead: Michael Avwerosuo Okeme
- Frontend: Ndabai Chiderah Daniella
