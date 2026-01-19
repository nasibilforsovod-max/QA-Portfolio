# Bug Report: BUG-004

## Bug Information

**ID:** BUG-004  
**Application:** Flow app  
**Version:** 1.1.9  
**Platform:** Android 13  
**Severity:** High  
**Priority:** High  
**Status:** Open  
**Found by:** nasibilforsovod@gmail.com  
**Date reported:** January 19, 2026

---

## Title
Poor Network Error Handling on Login - Technical Exception Exposed to User

---

## Description
When attempting to log in without an active internet connection, the application displays a raw technical exception message instead of a user-friendly error notification. The app shows "Exception: Login returned null token" rather than providing clear guidance like "No internet connection, please try again."

---

## Severity Justification
**High** - The application fails to handle a common scenario (no network connection) gracefully and provides poor user experience. While the app doesn't crash, users are blocked from proceeding and don't understand what action to take.

---

## Steps to Reproduce
1. Open Flow app (v1.1.9) on Android 13
2. Ensure device has no active internet connection (disable Wi-Fi and mobile data)
3. Navigate to the login screen
4. Observe that the "Sign in" button is active/enabled
5. Enter valid credentials
6. Tap the "Sign in" button

---

## Expected Result
- The app should detect the absence of network connection
- Display a user-friendly message such as:
  - "No internet connection detected"
  - "Please check your connection and try again"
- Provide clear guidance on what the user should do next
- Optionally: disable the Sign in button when no connection is available

---

## Actual Result
- The app attempts to perform login without network validation
- Displays technical exception text: "Exception: Login returned null token"
- User sees cryptic technical message with no actionable guidance
- User cannot proceed and doesn't understand the root cause

---

## Impact
- **User Experience:** Poor - users see confusing technical errors
- **Usability:** Blocks login functionality without clear instructions
- **User Trust:** May reduce confidence in application quality
- **Accessibility:** Non-technical users won't understand the error

---

## Additional Notes
- The Sign in button should ideally be disabled when no network is detected
- Network connectivity should be validated before attempting authentication
- All network-dependent operations should have proper error handling
- User-facing error messages should never expose raw exceptions

---

## Recommendations
1. Implement network connectivity check before login attempt
2. Replace technical exception with user-friendly error message
3. Add retry mechanism or guidance for users
4. Consider disabling network-dependent buttons when offline
5. Implement consistent error handling pattern across the app

---

## Attachments
- Screenshots (if available)
- Logs showing the exception
- Network state at time of error
