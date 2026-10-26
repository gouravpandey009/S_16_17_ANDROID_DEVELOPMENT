# S16–S17: Advanced Layout Design & Android UI Widgets
### Android Development | Kotlin + XML Views

This combined session focuses on designing clear Android interfaces, selecting suitable layouts, making screens adaptable, and using common UI widgets with Kotlin.

## Learning Outcomes

By the end of these sessions, students should be able to:

- Explain layout fundamentals and select a suitable layout for a screen.
- Use `LinearLayout`, `RelativeLayout`, and `ConstraintLayout`.
- Apply basic responsive UI principles.
- Display text, accept user input, respond to actions, show images, select options, and indicate progress.
- Connect XML views to Kotlin using IDs and event listeners.

## 1. Layout Fundamentals

Android layouts arrange and size views on a screen. A layout is commonly a `ViewGroup`; controls such as `TextView` and `Button` are views.

### Important attributes

| Attribute | Meaning |
|---|---|
| `match_parent` | Uses the available size of the parent in that dimension. |
| `wrap_content` | Sizes the view to fit its content. |
| `android:padding` | Space inside a view, between its content and boundary. |
| `android:layout_margin` | Space outside a view. |
| `dp` | Density-independent unit for dimensions and spacing. |
| `sp` | Scalable unit recommended for text sizes. |

**XML describes the interface; Kotlin handles its behaviour.**

## 2. Layout Selection Strategy

| Layout | How it works | Suitable use cases |
|---|---|---|
| `LinearLayout` | Arranges children vertically or horizontally. | Simple forms, stacked controls, button rows. |
| `RelativeLayout` | Positions views relative to the parent or other views. | Screens with straightforward relative positioning; useful when maintaining existing projects. |
| `ConstraintLayout` | Positions views using constraints to the parent or other views. | Flexible screens with multiple alignments and relationships. |

Choose the simplest layout that expresses the screen clearly. For new, moderately complex screens, `ConstraintLayout` is often a useful choice. Avoid unnecessary nesting and test the screen with different content sizes.

### A. LinearLayout example

```xml
<LinearLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:orientation="vertical"
    android:padding="16dp">

    <TextView
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="Student Profile"
        android:textSize="22sp" />

    <EditText
        android:id="@+id/etName"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:hint="Enter your name" />

    <Button
        android:id="@+id/btnSubmit"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="Submit" />
</LinearLayout>
```

Key attribute: `android:orientation` can be `vertical` or `horizontal`. `layout_weight` can distribute available space among children; use it deliberately.

### B. RelativeLayout example

```xml
<RelativeLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:padding="16dp">

    <TextView
        android:id="@+id/tvTitle"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Student Portal"
        android:textSize="22sp"
        android:layout_alignParentTop="true"
        android:layout_centerHorizontal="true" />

    <Button
        android:id="@+id/btnLogin"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Login"
        android:layout_below="@id/tvTitle"
        android:layout_centerHorizontal="true"
        android:layout_marginTop="20dp" />
</RelativeLayout>
```

Common rules include `layout_below`, `layout_above`, `layout_alignParentTop`, `layout_centerInParent`, and `layout_toEndOf`. Prefer start/end rules over left/right rules for better right-to-left language support.

### C. ConstraintLayout example

Ensure the project includes the ConstraintLayout dependency before using this snippet.

```xml
<androidx.constraintlayout.widget.ConstraintLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:padding="16dp">

    <TextView
        android:id="@+id/tvHeading"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Welcome"
        android:textSize="24sp"
        app:layout_constraintTop_toTopOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintEnd_toEndOf="parent" />

    <EditText
        android:id="@+id/etEmail"
        android:layout_width="0dp"
        android:layout_height="wrap_content"
        android:hint="Email address"
        android:layout_marginTop="24dp"
        app:layout_constraintTop_toBottomOf="@id/tvHeading"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintEnd_toEndOf="parent" />
</androidx.constraintlayout.widget.ConstraintLayout>
```

In `ConstraintLayout`, `0dp` in a constrained dimension means **match constraints**, not zero visible size. Views generally need enough horizontal and vertical constraints to determine their positions.

## 3. Responsive UI Design Basics

Responsive UI aims to keep a screen usable when available space or user settings change.

- Use `dp` for dimensions and spacing, and `sp` for text.
- Prefer flexible sizing and constraints over fixed screen-wide dimensions.
- Allow long text to wrap and use scrolling when appropriate.
- Avoid overlapping or clipped controls.
- Test portrait and landscape orientations, smaller and larger screens, and larger font settings.
- Consider alternate layouts/resources if the screen genuinely needs a different arrangement.

## 4. Android UI Widgets

### TextView — display text

```xml
<TextView
    android:id="@+id/tvMessage"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:text="Hello, Android!"
    android:textSize="18sp" />
```

Kotlin:
```kotlin
val tvMessage = findViewById<TextView>(R.id.tvMessage)
tvMessage.text = "Welcome back!"
```

### EditText — accept user input

```xml
<EditText
    android:id="@+id/etName"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:hint="Enter your name"
    android:inputType="textPersonName" />
```

Kotlin:
```kotlin
val name = findViewById<EditText>(R.id.etName).text.toString().trim()
```

Validate user input before processing it. Choose an `inputType` that suits the expected information.

### Button — trigger an action

```xml
<Button
    android:id="@+id/btnGreet"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:text="Greet" />
```

Kotlin:
```kotlin
val etName = findViewById<EditText>(R.id.etName)
val tvMessage = findViewById<TextView>(R.id.tvMessage)

findViewById<Button>(R.id.btnGreet).setOnClickListener {
    val name = etName.text.toString().trim()
    tvMessage.text =
        if (name.isEmpty()) "Please enter your name."
        else "Hello, $name!"
}
```

### ImageView — display an image

```xml
<ImageView
    android:id="@+id/ivLogo"
    android:layout_width="96dp"
    android:layout_height="96dp"
    android:src="@drawable/app_logo"
    android:contentDescription="@string/app_logo_description"
    android:scaleType="centerInside" />
```

Place the image resource in `res/drawable`. Use a meaningful content description when the image communicates information.

### CheckBox — multiple selection

```xml
<CheckBox
    android:id="@+id/cbAnnouncements"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:text="Receive announcements" />
```

Kotlin:
```kotlin
val receivesAnnouncements =
    findViewById<CheckBox>(R.id.cbAnnouncements).isChecked
```

Use `CheckBox` when users may select multiple independent options.

### RadioButton — single selection

Group mutually exclusive options in a `RadioGroup`:

```xml
<RadioGroup
    android:id="@+id/rgStudyMode"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:orientation="vertical">

    <RadioButton
        android:id="@+id/rbOnline"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Online" />

    <RadioButton
        android:id="@+id/rbOffline"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Offline" />
</RadioGroup>
```

Kotlin:
```kotlin
val selectedOptionId =
    findViewById<RadioGroup>(R.id.rgStudyMode).checkedRadioButtonId
```

### Switch — toggle a setting

```xml
<Switch
    android:id="@+id/swReminders"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:text="Enable reminders"
    android:checked="true" />
```

Kotlin:
```kotlin
findViewById<Switch>(R.id.swReminders)
    .setOnCheckedChangeListener { _, isEnabled ->
        // Apply or save the setting as appropriate.
    }
```

Use a switch for an on/off setting.

### Spinner — dropdown selection

```xml
<Spinner
    android:id="@+id/spCourse"
    android:layout_width="match_parent"
    android:layout_height="wrap_content" />
```

Kotlin:
```kotlin
val courses = listOf("CSE", "IT", "ECE")
val spinner = findViewById<Spinner>(R.id.spCourse)

val adapter = ArrayAdapter(
    this,
    android.R.layout.simple_spinner_item,
    courses
)
adapter.setDropDownViewResource(
    android.R.layout.simple_spinner_dropdown_item
)
spinner.adapter = adapter

spinner.onItemSelectedListener = object : AdapterView.OnItemSelectedListener {
    override fun onItemSelected(
        parent: AdapterView<*>, view: View?, position: Int, id: Long
    ) {
        val selectedCourse = courses[position]
    }

    override fun onNothingSelected(parent: AdapterView<*>) = Unit
}
```

Add the required imports for `ArrayAdapter`, `AdapterView`, `View`, and the widget classes. A `Spinner` uses an adapter to provide its items.

### ProgressBar — show progress

```xml
<ProgressBar
    android:id="@+id/progressLoading"
    style="?android:attr/progressBarStyleHorizontal"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:max="100"
    android:progress="0" />
```

Kotlin:
```kotlin
val progress = findViewById<ProgressBar>(R.id.progressLoading)
progress.progress = 60
```

Use a horizontal progress bar when progress can be measured. Use an indeterminate indicator when the duration or amount of work is unknown. Ensure progress feedback has a clear end state.

## 5. Widget Quick Reference

| Widget | Purpose | Example |
|---|---|---|
| `TextView` | Display text | Heading or result |
| `EditText` | Accept input | Name or email |
| `Button` | Start an action | Submit |
| `ImageView` | Display image | Logo or profile image |
| `CheckBox` | Multiple independent choices | Interests |
| `RadioButton` | One choice in a group | Study mode |
| `Switch` | Toggle a setting | Reminders |
| `Spinner` | Dropdown choice | Course |
| `ProgressBar` | Show progress/loading | Task progress |

## 6. Mini-Project: Student Profile & Preferences

### Goal
Build a single-screen app using Kotlin + XML Views. This is a UI practice task; no backend is required.

### Required components
- `TextView`: screen title and result summary.
- `EditText`: student name.
- `ImageView`: profile placeholder or local drawable.
- `RadioButton` in a `RadioGroup`: choose Online or Offline.
- `CheckBox`: opt in to announcements.
- `Switch`: enable/disable reminders.
- `Spinner`: choose a course or department.
- `Button`: submit the form.
- `ProgressBar`: demonstrate a progress state.

### Expected behaviour
When Submit is tapped:
1. Read and trim the student's name.
2. Show an error if the name is empty.
3. Read the selected study mode.
4. Read the checkbox and switch states.
5. Read the selected course from the Spinner.
6. Display a summary in the result `TextView`.
7. Show progress only while the simulated operation is in progress, then update or hide it.

Example output:
```text
Student: Asha
Study mode: Online
Course: CSE
Announcements: Yes
Reminders: On
```

### Extension tasks
- Add a Reset button.
- Handle the case where no study mode is selected.
- Try a different layout and compare the result.
- Test portrait and landscape orientations.
- Increase font size and fix any clipping.

## 7. Common Mistakes to Avoid

- Using a Kotlin view ID that does not exist in the XML.
- Forgetting to load the layout that contains the referenced view.
- Using input without validating it.
- Forgetting to group radio buttons when only one choice is allowed.
- Forgetting the Spinner adapter.
- Leaving a ProgressBar visible after the work is finished.
- Using fixed widths that cause clipping on smaller screens.
- Forgetting an appropriate content description for meaningful images.

## 8. Practice & Viva Questions

1. What is the difference between a `View` and a `ViewGroup`?
2. What is the difference between margin and padding?
3. When would you choose `LinearLayout` over `ConstraintLayout`?
4. How does `RelativeLayout` position its child views?
5. What does a constraint do in `ConstraintLayout`?
6. Why should text sizes use `sp`?
7. What is the difference between `CheckBox` and `RadioButton`?
8. Why does a `Spinner` need an adapter?
9. When should a `Switch` be used?
10. What is the difference between determinate and indeterminate progress?
11. How does a Button click listener work in Kotlin?
12. Why should UI be tested on different screen sizes and font settings?

## Final Checklist

- [ ] Layout choice is appropriate and can be explained.
- [ ] XML IDs match Kotlin references.
- [ ] Text uses readable sizes and is not clipped.
- [ ] Input is validated.
- [ ] CheckBox and RadioButton behaviours are correct.
- [ ] Spinner selection works.
- [ ] Switch responds to toggling.
- [ ] ProgressBar has clear start and end states.
- [ ] The screen has been tested on an emulator or physical device.

**Key takeaway:** A good Android interface combines a suitable layout, readable content, clear controls, validated input, and useful feedback.
