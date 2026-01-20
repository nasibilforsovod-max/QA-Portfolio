# Bug Report: BUG-006

## Bug Information

**ID:** BUG-006  
**Application:** ALPA Ukrainian educative games  
**Version:** 3.0.9  
**Platform:** Android 13  
**Severity:** Medium  
**Priority:** Medium  
**Status:** Open  
**Found by:** nasibilforsovod@gmail.com  
**Date reported:** January 20, 2026

---

## Title
Misleading Internet Toggle Behavior in Settings - Opens System Settings Instead of Toggling

---

## Description
The application settings contain an "Інтернет Вкл/Викл" (Internet On/Off) toggle switch. Instead of toggling the internet state directly within the app, clicking this toggle opens the system's network settings ("Сеть и интернет"). After returning to the app, the toggle changes its visual state, but the actual network state may remain unchanged, creating a misleading and confusing user experience.

---

## Severity Justification
**Medium** - This creates a confusing user experience through:
- Misleading UI behavior (toggle doesn't perform expected action)
- Visual state doesn't match actual state
- Users may believe they've changed network settings when they haven't
- Breaks standard toggle switch UX patterns
- Could lead to users playing with incorrect network settings

---

## Steps to Reproduce
1. Open ALPA Ukrainian educative games app (v3.0.9) on Android 13
2. Navigate to **Settings** (Настройки)
3. Locate the **"Інтернет Вкл/Викл"** toggle switch
4. Note the current toggle position (On or Off)
5. Tap/click on the toggle switch
6. Observe that system settings open (Сеть и интернет)
7. Press **Back** button to return to the app
8. Observe the toggle switch state in the app

---

## Expected Result
One of the following appropriate behaviors:

**Option 1 (Preferred):**
- Toggle switch directly controls internet connectivity for the app
- Visual state matches actual network state
- No redirection to system settings
- Immediate feedback of state change

**Option 2 (Alternative):**
- Clear notification/dialog explaining that user will be redirected to system settings
- Message like: "You will be redirected to system settings to change network configuration"
- Toggle state doesn't change until user actually modifies system settings
- Upon return, app detects and reflects actual network state

**Option 3 (Fallback):**
- Replace toggle with a button labeled "Open Network Settings"
- Make it clear this is a navigation action, not a toggle action

---

## Actual Result
- Tapping the toggle opens Android system settings (Network & Internet)
- User is taken out of the app context unexpectedly
- Upon returning to app, toggle visually changes position
- Actual network state may remain unchanged
- Toggle state doesn't accurately reflect real network status
- No warning or explanation provided
- User experience is confusing and misleading

---

## Impact
- **User Experience:** Confusing and misleading behavior
- **Usability:** Toggle doesn't perform expected action
- **Trust:** Users may lose confidence in app controls
- **Functionality:** Users might play with wrong network settings
- **UX Standards:** Violates common toggle switch design patterns

---

## Root Cause Analysis (Potential)
Possible technical/design issues:
- App lacks permission to control network state directly
- Developer implemented workaround by opening system settings
- Toggle state change is cosmetic, not functional
- App doesn't verify actual network state after user returns
- Missing state synchronization between app and system

---

## Additional Notes
- Toggle switch is a standard UI element that implies direct control
- Users expect toggles to change state immediately
- Opening system settings is unexpected behavior for a toggle
- Visual state change without actual state change is misleading
- Similar toggles in other apps work differently (direct control)

---

## User Confusion Scenarios
1. User taps toggle thinking internet is now off
2. System settings open, user presses back without changing anything
3. Toggle shows "Off" but internet is still on
4. User plays game thinking they're offline when they're not
5. Or vice versa - thinks they're online but they're offline

---

## Recommendations

### Short-term fixes:
1. **Replace toggle with button** - Use "Налаштування мережі" button instead of toggle
2. **Add confirmation dialog** - Warn user before opening system settings
3. **Sync state properly** - On return, check actual network state and update UI accordingly
4. **Add explanatory text** - Below toggle: "Opens system network settings"

### Long-term solutions:
1. **Request proper permissions** - Get permission to control network for the app
2. **Implement in-app control** - Add real toggle functionality without leaving app
3. **Use Android Network API** - Properly detect and reflect network state
4. **Follow UX guidelines** - Use appropriate UI element for the intended action

### Code suggestions:
```java
// Detect actual network state on resume
@Override
protected void onResume() {
    super.onResume();
    updateNetworkToggleState(); // Sync with actual state
}

// Or add clear navigation button instead
<Button
    android:text="Налаштування мережі"
    android:onClick="openNetworkSettings" />
```

---

## Design Pattern Violation
**Toggle Switch Pattern:**
- Should provide immediate binary state change
- Should show current actual state
- Should not require navigation to external screens
- Should not be used for navigation actions

**This implementation violates all these principles.**

---

## Test Data
- **Initial toggle state:** Both On and Off positions tested
- **Network state before:** Wi-Fi On, Mobile Data On
- **System settings action:** No changes made
- **Network state after:** Unchanged
- **Toggle visual state:** Changed despite no actual change
- **Reproducibility:** 100% (occurs every time)

---

## Attachments
- Screenshots of toggle in both states
- Screen recording showing system settings opening
- Before/after network state verification
- Comparison with similar toggles in other apps
