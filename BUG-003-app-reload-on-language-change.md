BUG-003-app-reload-on-language-change.md

## 📌 Summary

| Field | Value |
|---|---|
| **Bug ID** | BUG-003 |
| **Title** | Application Reloads on Language Change |
| **Application** | Свободний (Svobodniy) |
| **Version** | 4.5.2 |
| **Platform** | Android |
| **Device** | iHunt Titan Music P11000 PRO |
| **OS Version** | Android 13 |
| **Severity** | 🟢 Low |
| **Priority** | Low |
| **Type** | UX Issue / Performance |
| **Status** | Open |
| **Date Found** | January 19, 2026 |

---

## 📝 Description

When changing the language setting between Russian and Ukrainian in the app's Settings menu, the entire application reloads/restarts. This creates a poor user experience as the reload is unnecessary for a simple language change and causes the user to lose their current screen/context.

---

## 🔄 Steps to Reproduce

1. Launch the Svobodniy application
2. Navigate to Settings
3. Locate Language settings (currently showing Russian/Ukrainian options)
4. Select current language (e.g., Russian)
5. Change to the other language (e.g., Ukrainian)
6. Observe application behavior

---

## ✅ Expected Result

- Language should change smoothly without reloading
- UI text should update to the selected language
- User should remain on the Settings screen
- Application state should be preserved
- No noticeable delay or interruption
- Modern apps (like Telegram, WhatsApp) change language instantly

---

## ❌ Actual Result

- Application completely reloads/restarts
- User is returned to the initial screen (likely home/main screen)
- Current navigation context is lost
- Loading time is noticeable (app restart delay)
- Poor user experience compared to modern standards

---

## 🎯 Impact

**User Experience Issues:**
- Interrupts user workflow
- Causes unnecessary waiting time
- User loses current screen position
- Creates impression of unstable/poorly optimized app
- May discourage users from changing language settings

**Technical Concerns:**
- Indicates inefficient localization implementation
- Suggests app doesn't support runtime language switching
- May point to broader architecture issues

**Severity Justification:**
Low - Feature works correctly (language does change), but implementation is suboptimal. Does not prevent core functionality or cause data loss, but noticeably impacts user experience.

---

## 📸 Evidence

**Reproduction Rate:** 100% (occurs every time language is changed)

**Observed Behavior:**
1. User changes language in Settings
2. Screen goes blank/shows splash screen
3. App reloads completely
4. User lands on home screen instead of Settings

---

## 💡 Additional Notes

**Industry Standard:**
Most modern applications support **runtime language switching** without restart:
- Telegram - instant language change
- WhatsApp - instant language change  
- Google apps - instant language change
- Users expect this behavior in 2026

**Root Cause (Hypothesis):**
- App likely calls `recreate()` or similar method on language change
- Localization strings may be loaded only on app startup
- Missing proper runtime locale switching implementation

**Further Testing:**
- [x] Tested Russian → Ukrainian: App reloads
- [x] Tested Ukrainian → Russian: App reloads
- [ ] Check if any data/settings are lost during reload
- [ ] Test on different Android versions
- [ ] Compare behavior with similar apps in same category

**Recommendations:**

**Priority 1 - Implement Runtime Language Switching:**
```
Instead of:
- Detect language change → Reload entire app

Do:
- Detect language change → Update locale in runtime
- Refresh visible UI elements only
- Maintain current screen/navigation state
```

**Priority 2 - If Reload is Necessary:**
- Save current screen state before reload
- Restore user to the same screen after reload
- Show loading indicator to explain the delay
- Add smooth transition animation

**Priority 3 - Performance Optimization:**
- Minimize reload time if reload cannot be avoided
- Preload language resources to speed up switching

**Code Example (Conceptual):**
```kotlin
// BAD (current implementation):
fun onLanguageChanged() {
    setLocale(newLanguage)
    recreate() // Reloads entire activity
}

// GOOD (recommended):
fun onLanguageChanged() {
    setLocale(newLanguage)
    refreshUIStrings() // Update only UI elements
    // Stay on current screen
}
```

---

## 🔍 Similar Issues

This pattern is common in older Android apps but is considered outdated in modern development. Similar issues have been reported in:
- Legacy banking apps
- Older government apps
- Apps not updated to modern Android standards

**Best Practice Reference:**
- [Android Developer Guide: App Resources](https://developer.android.com/guide/topics/resources/localization)
- Modern apps should handle configuration changes gracefully

---

## 🏷️ Tags

`ux-issue` `performance` `localization` `language-switching` `android` `user-experience` `optimization` `low-severity`

---

## 📧 Reported By

**Tester:** nasibilforsovod@gmail.com  
**Date:** January 19, 2026

---

## 📊 Business Impact

**User Retention Risk:** Low  
Users may find this annoying but unlikely to uninstall the app solely for this reason.

**Fix Complexity:** Medium  
Requires refactoring localization implementation but is a well-documented solution in Android development.

**Priority for Fix:** Low-Medium  
Should be addressed in a future update as a UX improvement, but not urgent.
