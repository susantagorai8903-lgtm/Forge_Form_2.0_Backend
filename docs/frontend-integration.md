# Frontend integration contract

The Next.js API client reads `NEXT_PUBLIC_API_BASE_URL` as an absolute backend origin only. API endpoint paths include `/api/v1`, so do not add that path to the environment variable. The client sends:

- contact keys: `name`, `email`, `phone`, `program`, `goals`, `preferredContact`, `message`
- booking keys: `name`, `email`, `phone`, `program`, `date`, `time`, `goals`, `notes`

Set `NEXT_PUBLIC_API_BASE_URL=http://localhost:8000` locally. In Vercel, set it to the deployed backend origin (for example, `https://api.example.com`) and redeploy so the public variable is included in the frontend build. Set backend `FRONTEND_URL` to the exact frontend origin and `CORS_ORIGINS` to the comma-separated approved origins; `FRONTEND_URL` is also included in the CORS allowlist. Keep local development origins explicitly listed and do not use `*` in production.

The booking form submits `date` as the date-input value (`YYYY-MM-DD`) without converting it through UTC. It rejects past dates before submission, and the backend independently validates the date in `BOOKING_TIMEZONE`. Backend form responses intentionally retain `{id, message}`. Program, trainer, testimonial, and article public JSON is mapped to the existing TypeScript content shapes. The frontend currently renders local content data and does not call its content service functions; connecting those pages to API content should be a separate deliberate frontend change.

Do not treat the sample booking times rendered in the frontend as confirmed studio hours. Booking submissions are requests in `PENDING` status.
