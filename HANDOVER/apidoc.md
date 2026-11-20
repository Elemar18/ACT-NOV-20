
1. Document 1 Endpoint: Create a booking
POST /api/v1/bookings
Header: Authorization: Bearer <token>
Request:
{
  "guestId": "g-1024",
  "roomId": "r-305",
  "checkIn": "2026-12-01",
  "checkOut": "2026-12-04",
  "guests": 2
}
Response (201 Created):
{
  "bookingId": "b-88121",
  "status": "CONFIRMED",
  "roomId": "r-305",
  "checkIn": "2026-12-01",
  "checkOut": "2026-12-04",
  "totalNights": 3
}
Error response format:
{
  "error": {
    "code": "ROOM_UNAVAILABLE",
    "message": "Room r-305 is not available for the selected dates."
  }
}
HTTP status
Error code
Meaning
400
INVALID_REQUEST
A required field is missing or has the wrong format
400
INVALID_DATES
Check-out is not after check-in, or check-in is in the past
401
UNAUTHORIZED
Token is missing or invalid
404
ROOM_NOT_FOUND
The room ID does not exist
409
ROOM_UNAVAILABLE
The room is already booked for those dates
503
SERVICE_UNAVAILABLE
The database or message queue is down
500
INTERNAL_ERROR
Unexpected server error
Side effect: a successful booking publishes a booking.created message so the Housekeeping Service creates a cleaning task.
