# OnboardID SDK - Theming Guide

Customize the OnboardID SDK to match your brand identity. This guide covers all available theming options for both iOS and Android platforms.

---

### Quick Start

Apply a theme in a single call using `OnBoardIdUiV2.applyTheme()`. All properties are optional—only override what you need.

**Android (Kotlin):**
```kotlin
OnBoardIdUiV2.applyTheme(
    OnBoardIdTheme(
        buttons = OnBoardIdTheme.ButtonColors(
            primaryBackground = "#3B82F6",
            primaryText = "#FFFFFF"
        )
    )
)
```

**iOS (Swift):**
```swift
OnBoardIdUiV2.applyTheme(
    OnBoardIdTheme(
        buttons: OnBoardIdTheme.ButtonColors(
            primaryBackground: "#3B82F6",
            primaryText: "#FFFFFF"
        )
    )
)
```

---

## Table of Contents

- [Overview](#overview)
- [Button Styling](#button-styling)
- [Content Styling](#content-styling)
- [Instruction Screens](#instruction-screens)
- [Complete Examples](#complete-examples)
- [API Reference](#api-reference)

## Overview

The theming API allows you to customize:

- **Buttons** — Primary and secondary button colors, corner radius
- **Content** — Background, surface, border, and text colors
- **Instructions** — Custom text, images, videos, and step-by-step instructions for each capture screen

All colors use hex strings (e.g., `"#3B82F6"` or `"#FF3B82F6"` with alpha). Any property left `null`/`nil` uses the default SDK styling.

> **Platform Note:** The theming API is identical on iOS and Android. Code examples show both platforms for reference.

---

## Button Styling

Customize the appearance of primary and secondary buttons throughout the SDK.

### Primary Buttons

Primary buttons are used for main actions like "Continue" and "Take Photo".

**Android (Kotlin):**
```kotlin
OnBoardIdUiV2.applyTheme(
    OnBoardIdTheme(
        buttons = OnBoardIdTheme.ButtonColors(
            primaryBackground = "#3B82F6",
            primaryText = "#FFFFFF"
        )
    )
)
```

**iOS (Swift):**
```swift
OnBoardIdUiV2.applyTheme(
    OnBoardIdTheme(
        buttons: OnBoardIdTheme.ButtonColors(
            primaryBackground: "#3B82F6",
            primaryText: "#FFFFFF"
        )
    )
)
```

### Secondary Buttons

Secondary buttons are used for alternative actions like "Retake" and "Cancel".

**Android (Kotlin):**
```kotlin
OnBoardIdUiV2.applyTheme(
    OnBoardIdTheme(
        buttons = OnBoardIdTheme.ButtonColors(
            secondaryBackground = "#E5E7EB",
            secondaryText = "#374151"
        )
    )
)
```

**iOS (Swift):**
```swift
OnBoardIdUiV2.applyTheme(
    OnBoardIdTheme(
        buttons: OnBoardIdTheme.ButtonColors(
            secondaryBackground: "#E5E7EB",
            secondaryText: "#374151"
        )
    )
)
```
<table>
<tr>
<th>Default</th>
<th>Custom</th>
</tr>
<tr>
<td><img width="300" alt="Default Buttons" src="https://github.com/user-attachments/assets/9e921221-09d2-44ed-965f-498edf3c9dc4" /></td>
<td><img width="300" alt="Custom Buttons" src="https://github.com/user-attachments/assets/06da31f1-e560-4446-8030-0f21d780398e" /></td>
</tr>
</table>


### Corner Radius

Control the roundness of button corners. Values are in density-independent pixels (dp/points).

**Android (Kotlin):**
```kotlin
OnBoardIdUiV2.applyTheme(
    OnBoardIdTheme(
        buttons = OnBoardIdTheme.ButtonColors(
            cornerRadiusDp = 8f    // Slightly rounded
        )
    )
)
```

**iOS (Swift):**
```swift
OnBoardIdUiV2.applyTheme(
    OnBoardIdTheme(
        buttons: OnBoardIdTheme.ButtonColors(
            cornerRadiusDp: 8      // Slightly rounded
        )
    )
)
```

| Value | Effect |
|-------|--------|
| `0` | Square corners |
| `8` | Slightly rounded |
| `16` | Moderately rounded |
| `24+` | Pill-shaped buttons |

<table>
<tr>
<th>Default</th>
<th>Custom 0DP</th>
</tr>
<tr>
<td><img width="300" alt="Square Corners" src="https://github.com/user-attachments/assets/256964fc-6cc7-4bbe-bc4d-da8dd881db2f" /></td>
<td><img width="300" alt="Rounded Corners" src="https://github.com/user-attachments/assets/9d9393de-7014-4d91-a54d-7fc6d1b653f5" /></td>
</tr>
</table>


### Button Properties Reference

| Property | Type | Description |
|----------|------|-------------|
| `primaryBackground` | String (hex) | Background color for primary buttons |
| `primaryText` | String (hex) | Text color for primary buttons |
| `secondaryBackground` | String (hex) | Background color for secondary buttons |
| `secondaryText` | String (hex) | Text color for secondary buttons |
| `cornerRadiusDp` | Float/CGFloat | Corner radius in dp/points |

---

## Content Styling

Customize the colors of screens, cards, borders, and text throughout the SDK.

### Background and Surface Colors

- **Background** — The main screen background color
- **Surface** — Card and container background color
- **Border** — Border color for cards and input fields

**Android (Kotlin):**
```kotlin
OnBoardIdUiV2.applyTheme(
    OnBoardIdTheme(
        content = OnBoardIdTheme.ContentColors(
            background = "#F9FAFB",
            surface = "#FFFFFF",
            border = "#E5E7EB"
        )
    )
)
```

**iOS (Swift):**
```swift
OnBoardIdUiV2.applyTheme(
    OnBoardIdTheme(
        content: OnBoardIdTheme.ContentColors(
            background: "#F9FAFB",
            surface: "#FFFFFF",
            border: "#E5E7EB"
        )
    )
)
```

### Text Colors

Control the color of different text elements:

- **Title text** — Main headings
- **Subtitle text** — Secondary headings
- **Body text** — Paragraph and instruction text

**Android (Kotlin):**
```kotlin
OnBoardIdUiV2.applyTheme(
    OnBoardIdTheme(
        content = OnBoardIdTheme.ContentColors(
            titleText = "#111827",
            subtitleText = "#374151",
            bodyText = "#6B7280"
        )
    )
)
```

**iOS (Swift):**
```swift
OnBoardIdUiV2.applyTheme(
    OnBoardIdTheme(
        content: OnBoardIdTheme.ContentColors(
            titleText: "#111827",
            subtitleText: "#374151",
            bodyText: "#6B7280"
        )
    )
)
```

<table>
<tr>
<th>Default</th>
<th>Custom</th>
</tr>
<tr>
<td><img width="300" alt="Default Content" src="https://github.com/user-attachments/assets/0bea9bcc-9080-44e2-b129-ef515106af25" /></td>
<td><img width="300" alt="Custom Content" src="https://github.com/user-attachments/assets/547ded24-4964-424b-a9c9-cbc12fcfc019" /></td>
</tr>
</table>



### Content Properties Reference

| Property | Type | Description |
|----------|------|-------------|
| `background` | String (hex) | Main screen background color |
| `surface` | String (hex) | Card/container background color |
| `border` | String (hex) | Border color for cards and inputs |
| `titleText` | String (hex) | Color for main headings |
| `subtitleText` | String (hex) | Color for secondary headings |
| `bodyText` | String (hex) | Color for body/instruction text |

---

## Instruction Screens

Customize the instruction screens that appear before each capture step. Each screen is identified by a combination of `IdType` and `PictureType`.

### Targeting Specific Screens

Each instruction override targets a specific screen using `idType` and `pictureType`:

| IdType | PictureType | Screen |
|--------|-------------|--------|
| `DRIVERS_LICENSE` | `FRONT` | Driver's license front capture instructions |
| `DRIVERS_LICENSE` | `BACK` | Driver's license back capture instructions |
| `PASSPORT` | `FRONT` | Passport capture instructions |
| `GOVERNMENT_ID` | `FRONT` | Government ID front capture instructions |
| `GOVERNMENT_ID` | `BACK` | Government ID back capture instructions |
| `BIOMETRICS` | `SELFIE` | Selfie capture instructions |
| `BIOMETRICS` | `LIVENESS` | Liveness check instructions |

### Customizing Text

Override the title, subtitle, or body text for any instruction screen.

**Android (Kotlin):**
```kotlin
OnBoardIdUiV2.applyTheme(
    OnBoardIdTheme(
        instructions = listOf(
            OnBoardIdTheme.InstructionContent(
                idType = IdType.DRIVERS_LICENSE,
                pictureType = PictureType.FRONT,
                title = "Scan Your License",
                subtitle = "Front Side",
                body = "Position your license within the frame"
            )
        )
    )
)
```

**iOS (Swift):**
```swift
OnBoardIdUiV2.applyTheme(
    OnBoardIdTheme(
        instructions: [
            OnBoardIdTheme.InstructionContent(
                idType: .DRIVERS_LICENSE,
                pictureType: .FRONT,
                title: "Scan Your License",
                subtitle: "Front Side",
                body: "Position your license within the frame"
            )
        ]
    )
)
```

<table>
<tr>
<th>Default</th>
<th>Custom</th>
</tr>
<tr>
<td><img width="300" alt="Default Instructions" src="https://github.com/user-attachments/assets/e668291c-c620-4a3d-b2c4-04162f9233c0" /></td>
<td><img width="300" alt="Custom Instructions" src="https://github.com/user-attachments/assets/62e1d1df-1ea1-46c8-9338-143c05d53290" /></td>
</tr>
</table>


### Adding Instruction Steps

For more detailed instructions, use the `steps` property to display a list of items with optional icons.

**Android (Kotlin):**
```kotlin
// Build resource URIs for local icons
val packageName = packageName
val surfaceIconUrl = "android.resource://$packageName/${R.drawable.ic_instruction_surface}"
val cornersIconUrl = "android.resource://$packageName/${R.drawable.ic_instruction_corners}"
val lightIconUrl = "android.resource://$packageName/${R.drawable.ic_instruction_light}"

OnBoardIdUiV2.applyTheme(
    OnBoardIdTheme(
        instructions = listOf(
            OnBoardIdTheme.InstructionContent(
                idType = IdType.DRIVERS_LICENSE,
                pictureType = PictureType.FRONT,
                title = "Scan Your License",
                subtitle = "Follow these steps",
                steps = listOf(
                    OnBoardIdTheme.InstructionItem(
                        text = "Place on a flat, dark surface",
                        iconUrl = surfaceIconUrl
                    ),
                    OnBoardIdTheme.InstructionItem(
                        text = "Ensure all corners are visible",
                        iconUrl = cornersIconUrl
                    ),
                    OnBoardIdTheme.InstructionItem(
                        text = "Avoid glare and shadows",
                        iconUrl = lightIconUrl
                    )
                )
            )
        )
    )
)
```

**iOS (Swift):**
```swift
// Icons reference asset catalog names directly (no extension)
OnBoardIdUiV2.applyTheme(
    OnBoardIdTheme(
        instructions: [
            OnBoardIdTheme.InstructionContent(
                idType: .DRIVERS_LICENSE,
                pictureType: .FRONT,
                title: "Scan Your License",
                subtitle: "Follow these steps",
                steps: [
                    OnBoardIdTheme.InstructionItem(
                        text: "Place on a flat, dark surface",
                        iconUrl: "ic_instruction_surface"
                    ),
                    OnBoardIdTheme.InstructionItem(
                        text: "Ensure all corners are visible",
                        iconUrl: "ic_instruction_corners"
                    ),
                    OnBoardIdTheme.InstructionItem(
                        text: "Avoid glare and shadows",
                        iconUrl: "ic_instruction_light"
                    )
                ]
            )
        ]
    )
)
```

<table>
<tr>
<th>Default</th>
<th>Custom</th>
</tr>
<tr>
<td><img width="300" alt="Default Steps" src="https://github.com/user-attachments/assets/73f5f4e5-9597-47c0-8116-620d338f8649" /></td>
<td><img width="300" alt="Custom Steps" src="https://github.com/user-attachments/assets/fdacc8e0-3f5f-4d62-8ac6-a5a49d11bfa8" /></td>
</tr>
</table>



### Warning Steps

Mark a step as a warning to display it with amber/warning styling.

**Android (Kotlin):**
```kotlin
val warningIconUrl = "android.resource://$packageName/${R.drawable.ic_instruction_warning}"

OnBoardIdTheme.InstructionItem(
    text = "Do not use flash photography",
    iconUrl = warningIconUrl,
    isWarning = true
)
```

**iOS (Swift):**
```swift
OnBoardIdTheme.InstructionItem(
    text: "Do not use flash photography",
    iconUrl: "ic_instruction_warning",
    isWarning: true
)
```

<!-- Screenshot: instruction_warning_step.png - Warning step with amber background -->

### Custom Images and Videos

Replace the default instruction image or video for any screen.

**Android (Kotlin):**
```kotlin
// Build resource URIs for local image and video
val packageName = packageName
val passportImageUrl = "android.resource://$packageName/${R.drawable.passport_guide}"
val passportVideoPath = "android.resource://$packageName/${R.raw.passport_tutorial}"

OnBoardIdUiV2.applyTheme(
    OnBoardIdTheme(
        instructions = listOf(
            OnBoardIdTheme.InstructionContent(
                idType = IdType.PASSPORT,
                pictureType = PictureType.FRONT,
                title = "Scan Your Passport",
                imageUrl = passportImageUrl,
                videoPath = passportVideoPath
            )
        )
    )
)
```

**iOS (Swift):**
```swift
// Reference asset catalog names directly (no extension)
OnBoardIdUiV2.applyTheme(
    OnBoardIdTheme(
        instructions: [
            OnBoardIdTheme.InstructionContent(
                idType: .PASSPORT,
                pictureType: .FRONT,
                title: "Scan Your Passport",
                imageUrl: "passport_guide",
                videoPath: "passport_tutorial"
            )
        ]
    )
)
```

> **Note:** Both local resources and remote URLs are supported for images and videos. See the platform-specific formats below.

### Local Resource Formats

Icons and images can be loaded from local app resources or remote URLs. Each platform uses a different format for local resources:

**Android:**

Use the `android.resource://` URI format to reference drawable resources:

```kotlin
// Build resource URIs for local images and icons
val packageName = packageName
val iconUrl = "android.resource://$packageName/${R.drawable.ic_instruction_surface}"
val imageUrl = "android.resource://$packageName/${R.drawable.my_instruction_image}"

OnBoardIdUiV2.applyTheme(
    OnBoardIdTheme(
        instructions = listOf(
            OnBoardIdTheme.InstructionContent(
                idType = IdType.DRIVERS_LICENSE,
                pictureType = PictureType.FRONT,
                title = "Scan Your License",
                steps = listOf(
                    OnBoardIdTheme.InstructionItem(
                        text = "Place on a flat surface",
                        iconUrl = iconUrl
                    )
                ),
                imageUrl = imageUrl
            )
        )
    )
)
```

**iOS:**

Reference assets directly by their name in the asset catalog (without file extension):

```swift
OnBoardIdUiV2.applyTheme(
    OnBoardIdTheme(
        instructions: [
            OnBoardIdTheme.InstructionContent(
                idType: .DRIVERS_LICENSE,
                pictureType: .FRONT,
                title: "Scan Your License",
                steps: [
                    OnBoardIdTheme.InstructionItem(
                        text: "Place on a flat surface",
                        iconUrl: "ic_instruction_surface"  // Asset catalog name
                    )
                ],
                imageUrl: "my_instruction_image"  // Asset catalog name
            )
        ]
    )
)
```

| Platform | Local Resource Format | Example |
|----------|----------------------|---------|
| Android | `android.resource://{packageName}/{R.drawable.name}` | `"android.resource://com.example.app/${R.drawable.ic_surface}"` |
| iOS | Asset catalog name (no extension) | `"ic_surface"` |
| Both | Remote URL | `"https://example.com/icon.png"` |

### Instruction Properties Reference

**InstructionContent:**

| Property | Type | Description |
|----------|------|-------------|
| `idType` | IdType | Document type this instruction applies to |
| `pictureType` | PictureType | Capture step this instruction applies to |
| `title` | String? | Custom title text |
| `subtitle` | String? | Custom subtitle text |
| `body` | String? | Single body text (legacy, use `steps` instead) |
| `steps` | [InstructionItem]? | List of instruction steps with icons |
| `imageUrl` | String? | Custom instruction image (URL or local path) |
| `videoPath` | String? | Custom instruction video (URL or local path) |

**InstructionItem:**

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `text` | String | — | The instruction text to display |
| `iconUrl` | String? | `null` | URL or local path to an icon |
| `isWarning` | Boolean | `false` | Display with warning styling (amber background) |

---

## Complete Examples

### Full Brand Customization

Apply a complete theme with custom colors, buttons, and instructions.

**Android (Kotlin):**
```kotlin
// Build resource URIs for local drawable resources
val packageName = packageName
val surfaceIconUrl = "android.resource://$packageName/${R.drawable.ic_instruction_surface}"
val cornersIconUrl = "android.resource://$packageName/${R.drawable.ic_instruction_corners}"
val lightIconUrl = "android.resource://$packageName/${R.drawable.ic_instruction_light}"
val warningIconUrl = "android.resource://$packageName/${R.drawable.ic_instruction_warning}"
val faceIconUrl = "android.resource://$packageName/${R.drawable.ic_instruction_face}"
val dlFrontImageUrl = "android.resource://$packageName/${R.drawable.dl_front_guide}"

OnBoardIdUiV2.applyTheme(
    OnBoardIdTheme(
        buttons = OnBoardIdTheme.ButtonColors(
            primaryBackground = "#8C5A3C",   // Medium brown
            primaryText = "#FFF8F0",         // Cream
            secondaryBackground = "#FFF8F0", // Cream
            secondaryText = "#4B2E2B",       // Dark brown
            cornerRadiusDp = 12f
        ),
        content = OnBoardIdTheme.ContentColors(
            background = "#FFF8F0",   // Cream
            surface = "#F5EBE0",      // Warm light tan (cards)
            border = "#C08552",       // Caramel/tan
            titleText = "#4B2E2B",    // Dark brown
            subtitleText = "#8C5A3C", // Medium brown
            bodyText = "#4B2E2B"      // Dark brown
        ),
        instructions = listOf(
            OnBoardIdTheme.InstructionContent(
                idType = IdType.DRIVERS_LICENSE,
                pictureType = PictureType.FRONT,
                title = "Let's Verify Your Identity",
                subtitle = "Driver's License - Front",
                steps = listOf(
                    OnBoardIdTheme.InstructionItem(
                        text = "Place your ID on a dark, flat surface",
                        iconUrl = surfaceIconUrl
                    ),
                    OnBoardIdTheme.InstructionItem(
                        text = "Make sure all four corners are visible",
                        iconUrl = cornersIconUrl
                    ),
                    OnBoardIdTheme.InstructionItem(
                        text = "Avoid glare and shadows on the document",
                        iconUrl = lightIconUrl
                    ),
                    OnBoardIdTheme.InstructionItem(
                        text = "Do not cover any information with your fingers",
                        iconUrl = warningIconUrl,
                        isWarning = true
                    )
                ),
                imageUrl = dlFrontImageUrl
            ),
            OnBoardIdTheme.InstructionContent(
                idType = IdType.BIOMETRICS,
                pictureType = PictureType.SELFIE,
                title = "Almost Done!",
                subtitle = "Take a Selfie",
                steps = listOf(
                    OnBoardIdTheme.InstructionItem(
                        text = "Remove glasses, hats, and masks",
                        iconUrl = faceIconUrl
                    ),
                    OnBoardIdTheme.InstructionItem(
                        text = "Find good lighting - face a window if possible",
                        iconUrl = lightIconUrl
                    ),
                    OnBoardIdTheme.InstructionItem(
                        text = "Look straight at the camera",
                        iconUrl = faceIconUrl
                    )
                )
            )
        )
    )
)
```

**iOS (Swift):**
```swift
// Icons and images reference asset catalog names directly (no extension)
OnBoardIdUiV2.applyTheme(
    OnBoardIdTheme(
        buttons: OnBoardIdTheme.ButtonColors(
            primaryBackground: "#8C5A3C",   // Medium brown
            primaryText: "#FFF8F0",         // Cream
            secondaryBackground: "#FFF8F0", // Cream
            secondaryText: "#4B2E2B",       // Dark brown
            cornerRadiusDp: 12
        ),
        content: OnBoardIdTheme.ContentColors(
            background: "#FFF8F0",   // Cream
            surface: "#F5EBE0",      // Warm light tan (cards)
            border: "#C08552",       // Caramel/tan
            titleText: "#4B2E2B",    // Dark brown
            subtitleText: "#8C5A3C", // Medium brown
            bodyText: "#4B2E2B"      // Dark brown
        ),
        instructions: [
            OnBoardIdTheme.InstructionContent(
                idType: .DRIVERS_LICENSE,
                pictureType: .FRONT,
                title: "Let's Verify Your Identity",
                subtitle: "Driver's License - Front",
                steps: [
                    OnBoardIdTheme.InstructionItem(
                        text: "Place your ID on a dark, flat surface",
                        iconUrl: "ic_instruction_surface"
                    ),
                    OnBoardIdTheme.InstructionItem(
                        text: "Make sure all four corners are visible",
                        iconUrl: "ic_instruction_corners"
                    ),
                    OnBoardIdTheme.InstructionItem(
                        text: "Avoid glare and shadows on the document",
                        iconUrl: "ic_instruction_light"
                    ),
                    OnBoardIdTheme.InstructionItem(
                        text: "Do not cover any information with your fingers",
                        iconUrl: "ic_instruction_warning",
                        isWarning: true
                    )
                ],
                imageUrl: "dl_front_guide"
            ),
            OnBoardIdTheme.InstructionContent(
                idType: .BIOMETRICS,
                pictureType: .SELFIE,
                title: "Almost Done!",
                subtitle: "Take a Selfie",
                steps: [
                    OnBoardIdTheme.InstructionItem(
                        text: "Remove glasses, hats, and masks",
                        iconUrl: "ic_instruction_face"
                    ),
                    OnBoardIdTheme.InstructionItem(
                        text: "Find good lighting - face a window if possible",
                        iconUrl: "ic_instruction_light"
                    ),
                    OnBoardIdTheme.InstructionItem(
                        text: "Look straight at the camera",
                        iconUrl: "ic_instruction_face"
                    )
                ]
            )
        ]
    )
)
```

<table>
<tr>
<th>Default</th>
<th>Custom</th>
</tr>
<tr>
<td><img width="300" alt="Default ID Instructions" src="https://github.com/user-attachments/assets/b6b3135b-2880-4a51-a90b-a23986b3aa89" /></td>
<td><img width="300" alt="Custom ID Instructions" src="https://github.com/user-attachments/assets/3a373a1c-e754-46eb-8c2b-800db7d16319" /></td>
</tr>
<tr>
<td><img width="300" alt="Default Capture Screen" src="https://github.com/user-attachments/assets/91916523-0733-4bdf-96d1-91350b96db11" /></td>
<td><img width="300" alt="Custom Capture Screen" src="https://github.com/user-attachments/assets/ff175e9e-eada-4529-be60-ba3b353b77a3" /></td>
</tr>
<tr>
<td><img width="300" alt="Default Review Screen" src="https://github.com/user-attachments/assets/3d0e611d-18b3-4aca-9ad6-de108fa704c9" /></td>
<td><img width="300" alt="Custom Review Screen" src="https://github.com/user-attachments/assets/dc4233a3-4cdc-4085-a1db-8898432b463d" /></td>
</tr>
</table>

---

## API Reference

### OnBoardIdUiV2

Entry point for theming the SDK.

| Method | Description |
|--------|-------------|
| `applyTheme(theme)` | Apply a complete theme configuration |
| `enable()` | Enable V2 UI (only needed if you previously switched to V1) |

### OnBoardIdTheme

Top-level theme configuration.

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `buttons` | ButtonColors | `ButtonColors()` | Button styling configuration |
| `content` | ContentColors | `ContentColors()` | Content/surface styling configuration |
| `instructions` | [InstructionContent] | `[]` | Instruction screen overrides |

### ButtonColors

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `primaryBackground` | String? | `null` | Primary button background color (hex) |
| `primaryText` | String? | `null` | Primary button text color (hex) |
| `secondaryBackground` | String? | `null` | Secondary button background color (hex) |
| `secondaryText` | String? | `null` | Secondary button text color (hex) |
| `cornerRadiusDp` | Float/CGFloat? | `null` | Button corner radius in dp/points |

### ContentColors

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `background` | String? | `null` | Screen background color (hex) |
| `surface` | String? | `null` | Card/container background color (hex) |
| `border` | String? | `null` | Border color (hex) |
| `titleText` | String? | `null` | Title text color (hex) |
| `subtitleText` | String? | `null` | Subtitle text color (hex) |
| `bodyText` | String? | `null` | Body text color (hex) |

### InstructionContent

| Property | Type | Required | Description |
|----------|------|----------|-------------|
| `idType` | IdType | Yes | Document type |
| `pictureType` | PictureType | Yes | Capture step |
| `title` | String? | No | Custom title |
| `subtitle` | String? | No | Custom subtitle |
| `body` | String? | No | Body text (legacy) |
| `steps` | [InstructionItem]? | No | Instruction steps (takes precedence over `body`) |
| `imageUrl` | String? | No | Custom image URL or path |
| `videoPath` | String? | No | Custom video URL or path |

### InstructionItem

| Property | Type | Required | Default | Description |
|----------|------|----------|---------|-------------|
| `text` | String | Yes | — | Instruction text |
| `iconUrl` | String? | No | `null` | Icon URL or path |
| `isWarning` | Boolean | No | `false` | Show with warning styling |

### IdType + PictureType Combinations

| IdType | Valid PictureTypes | Description |
|--------|-------------------|-------------|
| `DRIVERS_LICENSE` | `FRONT`, `BACK` | Driver's license capture |
| `PASSPORT` | `FRONT`, `EPASSPORT` | Passport capture |
| `GOVERNMENT_ID` | `FRONT`, `BACK` | National ID capture |
| `BIOMETRICS` | `SELFIE`, `LIVENESS` | Biometric capture |

### IdType Values

| Value | Android | iOS | Description |
|-------|---------|-----|-------------|
| Driver's License | `IdType.DRIVERS_LICENSE` | `.DRIVERS_LICENSE` | Driver's license |
| Passport | `IdType.PASSPORT` | `.PASSPORT` | Passport |
| Government ID | `IdType.GOVERNMENT_ID` | `.GOVERNMENT_ID` | National ID card |
| Biometrics | `IdType.BIOMETRICS` | `.BIOMETRICS` | Selfie/liveness |

### PictureType Values

| Value | Android | iOS | Description |
|-------|---------|-----|-------------|
| Front | `PictureType.FRONT` | `.FRONT` | Document front |
| Back | `PictureType.BACK` | `.BACK` | Document back |
| Selfie | `PictureType.SELFIE` | `.SELFIE` | Selfie capture |
| Liveness | `PictureType.LIVENESS` | `.LIVENESS` | Liveness check |
| E-Passport | `PictureType.EPASSPORT` | `.EPASSPORT` | NFC passport |
