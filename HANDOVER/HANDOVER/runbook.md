2. Author 1 Runbook Entry: Restart the Booking API
When to use: /health fails or does not respond, or bookings return 5xx errors.
Run all commands from the project folder that contains docker-compose.yml.
1.  Confirm the problem: run curl -i http://localhost:8080/health. A healthy response is 200 with {"status":"ok","database":"up","queue":"up"}. No response or a 5xx means continue.
2.  Check the dependencies: run docker compose ps. The database and the queue must show as running. If either is down, start it first (see Sequence Services), then continue.
3.  Save the logs: run docker compose logs --tail=100 booking-api > booking-api-before-restart.log.
4.  Restart: run docker compose restart booking-api.
5.  Wait 30 seconds, then run the health command from step 1. Expected: 200 with database and queue "up".
6.  Smoke test: create a test booking with POST /api/v1/bookings. Expected: 201, and a housekeeping task appears within 5 seconds.
7.  Escalate: if /health still fails after two restarts, stop and send booking-api-before-restart.log to the Project Lead. Do not keep restarting.