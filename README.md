# StudyHub: Common Screens App (Java + XML)

An Android app (Java, XML layouts, minSdk 24) that implements the common screens of a typical app:
splash, login, registration, password recovery, a module dashboard and a user profile with logout.
There is **no backend**: authentication and password recovery are simulated locally, as allowed by the assignment.

Student: Ghulam Haider | Reg. No.: 24-Arid-1027 | UIIT, UAAR

## Demo account

| Email | Username | Password |
|---|---|---|
| `demo@studyhub.com` | `demo` | `Demo@123` |

You can also create your own account on the Register screen.

## Screens and features

| # | Screen | Activity | What it does |
|---|---|---|---|
| 1 | Splash | `SplashActivity` | Shows logo, app name and tagline for 2 seconds, then opens Login. |
| 2 | Login | `LoginActivity` | Email **or** username + password, show/hide password, Login button, Register button, Forgot Password link. Shows an error for wrong credentials. |
| 3 | Register | `RegisterActivity` | Name, email, password, confirm password. Validates every field and blocks duplicate emails. |
| 4 | Forgot Password | `ForgotPasswordActivity` | Accepts an email, checks it exists and **simulates** sending a reset link. |
| 5 | Reset Password | `ResetPasswordActivity` | New password + confirm password with validation; updates the saved password. |
| 6 | Dashboard | `DashboardActivity` | Six module cards in a grid (Courses, Assignments, Schedule, Grades, Notifications, Profile), each with an icon and text. |
| 7 | Profile | `ProfileActivity` | Shows name, email, username and status; **Logout** button. |

### Validation rules
- Name: required, at least 3 characters.
- Email: required, valid email format (`Patterns.EMAIL_ADDRESS`), must not already be registered.
- Password: required, at least 6 characters, at least one letter and one digit.
- Confirm password: required and must match.

## Navigation flow

```
Splash --(2 s)--> Login --Login OK--> Dashboard --Profile card--> Profile --Logout--> Login
                    |  \
                    |   `--Forgot Password--> Forgot Password --(simulated link)--> Reset Password --Save--> Login
                    `--Register--> Register --Create Account--> back to Login
```
All navigation uses **explicit Intents**. After login the Login screen is removed from the back stack, and Logout / Reset Password clear the back stack so the Back button never returns to a protected screen.

## Technical requirements covered

| Requirement | Where |
|---|---|
| Default resources | `res/values/strings.xml`, `colors.xml`, `themes.xml`, `auth_styles.xml` |
| Alternative resources: **night mode colors** | `res/values-night/colors.xml` (+ `themes.xml`); DayNight theme switches automatically |
| Alternative resources: **Urdu strings** | `res/values-ur/strings.xml` (used when the phone language is Urdu; layout supports RTL) |
| LinearLayout | Register, Forgot Password, Reset Password, Profile (and the containers of Dashboard) |
| GridLayout | `activity_dashboard.xml` (2 x 3 module cards) |
| ConstraintLayout | `activity_splash.xml`, `activity_login.xml` |
| Text / email / password inputs | `inputType`: `text`, `textPersonName`, `textEmailAddress`, `textPassword` |
| Button clicks and validation | `Validator.java` and the click listeners in each Activity |
| Explicit Intents | `new Intent(this, X.class)` in every Activity |
| Consistent UI | shared styles (`AuthHeader`, `AuthInput`, `AuthButton`...) and one blue/white palette |

## How the simulated authentication works
`UserStore.java` keeps registered users and the current session in `SharedPreferences` (as JSON text).
Passwords are stored as SHA-256 hashes. The demo account is created the first time the app runs.
This is for learning only; a real app needs a secure server and salted password hashing.

## Project structure
```
app/src/main
 +-- AndroidManifest.xml
 +-- java/com/uiit/studyhub
 |     SplashActivity, LoginActivity, RegisterActivity, ForgotPasswordActivity,
 |     ResetPasswordActivity, DashboardActivity, ProfileActivity,
 |     User, UserStore, Validator
 +-- res
       layout/           7 screens
       drawable/         logo, module icons, backgrounds
       values/           strings, colors, themes, styles       (default)
       values-night/     colors, themes                         (dark mode)
       values-ur/        strings                                (Urdu)
```

## Run it
1. Open the project in Android Studio and let Gradle sync.
2. Run on an emulator or a phone (Android 7.0 / API 24 or newer).
3. Log in with the demo account above, or register a new one.
4. To test the alternative resources: turn on dark theme in the phone settings (night colors), or change the phone language to Urdu (Urdu strings).

## Tech
Java, XML, AndroidX AppCompat, ConstraintLayout. No other libraries.
