# OnboardID SDK - Theming Guide

Customize the OnboardID SDK to match your brand identity. This guide covers all available theming options for iOS, Android, Flutter, and React Native platforms.

---

### Quick Start

Apply a theme in a single call. All properties are optional—only override what you need.

---

<details>
<summary><code><b>Android</b></code> Kotlin <small><i>· click to expand</i></small></summary>

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
</details>

<details>
<summary><code><b>iOS</b></code> Swift <small><i>· click to expand</i></small></summary>

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
</details>

<details>
<summary><code><b>Flutter</b></code> Dart <small><i>· click to expand</i></small></summary>

```dart
await NetkiSdk.instance.applyTheme(
    theme: OnBoardIdTheme(
        buttons: ButtonColors(
            primaryBackground: '#3B82F6',
            primaryText: '#FFFFFF',
        ),
    ),
);
```
</details>

<details>
<summary><code><b>React Native</b></code> TypeScript <small><i>· click to expand</i></small></summary>

```typescript
import netkiSDK from '@netki/netki-mobilesdk';
import type { OnBoardIdTheme } from '@netki/netki-mobilesdk';

const theme: OnBoardIdTheme = {
    buttons: {
        primaryBackground: '#3B82F6',
        primaryText: '#FFFFFF',
    },
};

await netkiSDK.applyTheme(JSON.stringify(theme));
```
</details>

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

> **Platform Note:** The theming API is identical across iOS, Android, Flutter, and React Native. Code examples show all platforms for reference.

---

## Button Styling

Customize the appearance of primary and secondary buttons throughout the SDK.

### Primary Buttons

Primary buttons are used for main actions like "Continue" and "Take Photo".

---

<details>
<summary><code><b>Android</b></code> Kotlin <small><i>· click to expand</i></small></summary>

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

</details>

<details>
<summary><code><b>iOS</b></code> Swift <small><i>· click to expand</i></small></summary>

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

</details>

<details>
<summary><code><b>Flutter</b></code> Dart <small><i>· click to expand</i></small></summary>

```dart
await NetkiSdk.instance.applyTheme(
    theme: OnBoardIdTheme(
        buttons: ButtonColors(
            primaryBackground: '#3B82F6',
            primaryText: '#FFFFFF',
        ),
    ),
);
```

</details>

<details>
<summary><code><b>React Native</b></code> TypeScript <small><i>· click to expand</i></small></summary>

```typescript
const theme: OnBoardIdTheme = {
    buttons: {
        primaryBackground: '#3B82F6',
        primaryText: '#FFFFFF',
    },
};

await netkiSDK.applyTheme(JSON.stringify(theme));
```

</details>

---

### Secondary Buttons

Secondary buttons are used for alternative actions like "Retake" and "Cancel".

---

<details>
<summary><code><b>Android</b></code> Kotlin <small><i>· click to expand</i></small></summary>

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

</details>

<details>
<summary><code><b>iOS</b></code> Swift <small><i>· click to expand</i></small></summary>

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

</details>

<details>
<summary><code><b>Flutter</b></code> Dart <small><i>· click to expand</i></small></summary>

```dart
await NetkiSdk.instance.applyTheme(
    theme: OnBoardIdTheme(
        buttons: ButtonColors(
            secondaryBackground: '#E5E7EB',
            secondaryText: '#374151',
        ),
    ),
);
```

</details>

<details>
<summary><code><b>React Native</b></code> TypeScript <small><i>· click to expand</i></small></summary>

```typescript
const theme: OnBoardIdTheme = {
    buttons: {
        secondaryBackground: '#E5E7EB',
        secondaryText: '#374151',
    },
};

await netkiSDK.applyTheme(JSON.stringify(theme));
```

</details>

---

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

---

<details>
<summary><code><b>Android</b></code> Kotlin <small><i>· click to expand</i></small></summary>

```kotlin
OnBoardIdUiV2.applyTheme(
    OnBoardIdTheme(
        buttons = OnBoardIdTheme.ButtonColors(
            cornerRadiusDp = 8f    // Slightly rounded
        )
    )
)
```

</details>

<details>
<summary><code><b>iOS</b></code> Swift <small><i>· click to expand</i></small></summary>

```swift
OnBoardIdUiV2.applyTheme(
    OnBoardIdTheme(
        buttons: OnBoardIdTheme.ButtonColors(
            cornerRadiusDp: 8      // Slightly rounded
        )
    )
)
```

</details>

<details>
<summary><code><b>Flutter</b></code> Dart <small><i>· click to expand</i></small></summary>

```dart
await NetkiSdk.instance.applyTheme(
    theme: OnBoardIdTheme(
        buttons: ButtonColors(
            cornerRadiusDp: 8,     // Slightly rounded
        ),
    ),
);
```

</details>

<details>
<summary><code><b>React Native</b></code> TypeScript <small><i>· click to expand</i></small></summary>

```typescript
const theme: OnBoardIdTheme = {
    buttons: {
        cornerRadiusDp: 8,     // Slightly rounded
    },
};

await netkiSDK.applyTheme(JSON.stringify(theme));
```

</details>

---

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

---

<details>
<summary><code><b>Android</b></code> Kotlin <small><i>· click to expand</i></small></summary>

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

</details>

<details>
<summary><code><b>iOS</b></code> Swift <small><i>· click to expand</i></small></summary>

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

</details>

<details>
<summary><code><b>Flutter</b></code> Dart <small><i>· click to expand</i></small></summary>

```dart
await NetkiSdk.instance.applyTheme(
    theme: OnBoardIdTheme(
        content: ContentColors(
            background: '#F9FAFB',
            surface: '#FFFFFF',
            border: '#E5E7EB',
        ),
    ),
);
```

</details>

<details>
<summary><code><b>React Native</b></code> TypeScript <small><i>· click to expand</i></small></summary>

```typescript
const theme: OnBoardIdTheme = {
    content: {
        background: '#F9FAFB',
        surface: '#FFFFFF',
        border: '#E5E7EB',
    },
};

await netkiSDK.applyTheme(JSON.stringify(theme));
```

</details>

---

### Text Colors

Control the color of different text elements:

- **Title text** — Main headings
- **Subtitle text** — Secondary headings
- **Body text** — Paragraph and instruction text

---

<details>
<summary><code><b>Android</b></code> Kotlin <small><i>· click to expand</i></small></summary>

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

</details>

<details>
<summary><code><b>iOS</b></code> Swift <small><i>· click to expand</i></small></summary>

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

</details>

<details>
<summary><code><b>Flutter</b></code> Dart <small><i>· click to expand</i></small></summary>

```dart
await NetkiSdk.instance.applyTheme(
    theme: OnBoardIdTheme(
        content: ContentColors(
            titleText: '#111827',
            subtitleText: '#374151',
            bodyText: '#6B7280',
        ),
    ),
);
```

</details>

<details>
<summary><code><b>React Native</b></code> TypeScript <small><i>· click to expand</i></small></summary>

```typescript
const theme: OnBoardIdTheme = {
    content: {
        titleText: '#111827',
        subtitleText: '#374151',
        bodyText: '#6B7280',
    },
};

await netkiSDK.applyTheme(JSON.stringify(theme));
```

</details>

---

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

---

<details>
<summary><code><b>Android</b></code> Kotlin <small><i>· click to expand</i></small></summary>

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

</details>

<details>
<summary><code><b>iOS</b></code> Swift <small><i>· click to expand</i></small></summary>

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

</details>

<details>
<summary><code><b>Flutter</b></code> Dart <small><i>· click to expand</i></small></summary>

```dart
await NetkiSdk.instance.applyTheme(
    theme: OnBoardIdTheme(
        instructions: [
            InstructionContent(
                idType: IdType.driversLicense,
                pictureType: PictureType.front,
                title: 'Scan Your License',
                subtitle: 'Front Side',
                body: 'Position your license within the frame',
            ),
        ],
    ),
);
```

</details>

<details>
<summary><code><b>React Native</b></code> TypeScript <small><i>· click to expand</i></small></summary>

```typescript
const theme: OnBoardIdTheme = {
    instructions: [
        {
            idType: 'DRIVERS_LICENSE',
            pictureType: 'FRONT',
            title: 'Scan Your License',
            subtitle: 'Front Side',
            body: 'Position your license within the frame',
        },
    ],
};

await netkiSDK.applyTheme(JSON.stringify(theme));
```

</details>

---

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

---

<details>
<summary><code><b>Android</b></code> Kotlin <small><i>· click to expand</i></small></summary>

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

</details>

<details>
<summary><code><b>iOS</b></code> Swift <small><i>· click to expand</i></small></summary>

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

</details>

<details>
<summary><code><b>Flutter</b></code> Dart <small><i>· click to expand</i></small></summary>

```dart
// Resolve Flutter assets to file paths first
final paths = await NetkiSdk.instance.resolveAssetPaths([
    'assets/icons/ic_instruction_surface.png',
    'assets/icons/ic_instruction_corners.png',
    'assets/icons/ic_instruction_light.png',
]);

await NetkiSdk.instance.applyTheme(
    theme: OnBoardIdTheme(
        instructions: [
            InstructionContent(
                idType: IdType.driversLicense,
                pictureType: PictureType.front,
                title: 'Scan Your License',
                subtitle: 'Follow these steps',
                steps: [
                    InstructionItem(
                        text: 'Place on a flat, dark surface',
                        iconUrl: paths['assets/icons/ic_instruction_surface.png'],
                    ),
                    InstructionItem(
                        text: 'Ensure all corners are visible',
                        iconUrl: paths['assets/icons/ic_instruction_corners.png'],
                    ),
                    InstructionItem(
                        text: 'Avoid glare and shadows',
                        iconUrl: paths['assets/icons/ic_instruction_light.png'],
                    ),
                ],
            ),
        ],
    ),
);
```

</details>

<details>
<summary><code><b>React Native</b></code> TypeScript <small><i>· click to expand</i></small></summary>

```typescript
import { Image } from 'react-native';

// Import assets using require()
const icons = {
    surface: require('./assets/icons/ic_instruction_surface.png'),
    corners: require('./assets/icons/ic_instruction_corners.png'),
    light: require('./assets/icons/ic_instruction_light.png'),
};

// Resolve to URIs using Image.resolveAssetSource()
const getAssetUri = (source: any): string => {
    const resolved = Image.resolveAssetSource(source);
    return resolved?.uri || '';
};

const theme: OnBoardIdTheme = {
    instructions: [
        {
            idType: 'DRIVERS_LICENSE',
            pictureType: 'FRONT',
            title: 'Scan Your License',
            subtitle: 'Follow these steps',
            steps: [
                {
                    text: 'Place on a flat, dark surface',
                    iconUrl: getAssetUri(icons.surface),
                },
                {
                    text: 'Ensure all corners are visible',
                    iconUrl: getAssetUri(icons.corners),
                },
                {
                    text: 'Avoid glare and shadows',
                    iconUrl: getAssetUri(icons.light),
                },
            ],
        },
    ],
};

await netkiSDK.applyTheme(JSON.stringify(theme));
```

</details>

---

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

---

<details>
<summary><code><b>Android</b></code> Kotlin <small><i>· click to expand</i></small></summary>

```kotlin
val warningIconUrl = "android.resource://$packageName/${R.drawable.ic_instruction_warning}"

OnBoardIdTheme.InstructionItem(
    text = "Do not use flash photography",
    iconUrl = warningIconUrl,
    isWarning = true
)
```

</details>

<details>
<summary><code><b>iOS</b></code> Swift <small><i>· click to expand</i></small></summary>

```swift
OnBoardIdTheme.InstructionItem(
    text: "Do not use flash photography",
    iconUrl: "ic_instruction_warning",
    isWarning: true
)
```

</details>

<details>
<summary><code><b>Flutter</b></code> Dart <small><i>· click to expand</i></small></summary>

```dart
final warningIconPath = await NetkiSdk.instance.resolveAssetPath(
    'assets/icons/ic_instruction_warning.png',
);

InstructionItem(
    text: 'Do not use flash photography',
    iconUrl: warningIconPath,
    isWarning: true,
)
```

</details>

<details>
<summary><code><b>React Native</b></code> TypeScript <small><i>· click to expand</i></small></summary>

```typescript
const warningIcon = require('./assets/icons/ic_instruction_warning.png');

{
    text: 'Do not use flash photography',
    iconUrl: getAssetUri(warningIcon),
    isWarning: true,
}
```

</details>

---

<!-- Screenshot: instruction_warning_step.png - Warning step with amber background -->

### Custom Images and Videos

Replace the default instruction image or video for any screen.

---

<details>
<summary><code><b>Android</b></code> Kotlin <small><i>· click to expand</i></small></summary>

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

</details>

<details>
<summary><code><b>iOS</b></code> Swift <small><i>· click to expand</i></small></summary>

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

</details>

<details>
<summary><code><b>Flutter</b></code> Dart <small><i>· click to expand</i></small></summary>

```dart
// Resolve Flutter assets to file paths
final paths = await NetkiSdk.instance.resolveAssetPaths([
    'assets/images/passport_guide.png',
    'assets/videos/passport_tutorial.mp4',
]);

await NetkiSdk.instance.applyTheme(
    theme: OnBoardIdTheme(
        instructions: [
            InstructionContent(
                idType: IdType.passport,
                pictureType: PictureType.front,
                title: 'Scan Your Passport',
                imageUrl: paths['assets/images/passport_guide.png'],
                videoPath: paths['assets/videos/passport_tutorial.mp4'],
            ),
        ],
    ),
);
```

</details>

<details>
<summary><code><b>React Native</b></code> TypeScript <small><i>· click to expand</i></small></summary>

```typescript
const passportGuide = require('./assets/images/passport_guide.png');

const theme: OnBoardIdTheme = {
    instructions: [
        {
            idType: 'PASSPORT',
            pictureType: 'FRONT',
            title: 'Scan Your Passport',
            imageUrl: getAssetUri(passportGuide),
            // Note: videoPath is also supported
        },
    ],
};

await netkiSDK.applyTheme(JSON.stringify(theme));
```

</details>

---

> **Note:** Both local resources and remote URLs are supported for images and videos. See the platform-specific formats below.

### Local Resource Formats

Icons and images can be loaded from local app resources or remote URLs. Each platform uses a different format for local resources:

---

<details>
<summary><code><b>Android</b></code> <small><i>· click to expand</i></small></summary>

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

</details>

<details>
<summary><code><b>iOS</b></code> <small><i>· click to expand</i></small></summary>

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

</details>

<details>
<summary><code><b>Flutter</b></code> <small><i>· click to expand</i></small></summary>

Flutter assets must be resolved to file paths using `resolveAssetPath()` or `resolveAssetPaths()` before passing to the theme. This is because the native SDK cannot directly access Flutter's asset bundle.

```dart
// Single asset
final iconPath = await NetkiSdk.instance.resolveAssetPath(
    'assets/icons/ic_instruction_surface.png',
);

// Multiple assets (more efficient)
final paths = await NetkiSdk.instance.resolveAssetPaths([
    'assets/icons/ic_instruction_surface.png',
    'assets/images/my_instruction_image.png',
]);

await NetkiSdk.instance.applyTheme(
    theme: OnBoardIdTheme(
        instructions: [
            InstructionContent(
                idType: IdType.driversLicense,
                pictureType: PictureType.front,
                title: 'Scan Your License',
                steps: [
                    InstructionItem(
                        text: 'Place on a flat surface',
                        iconUrl: paths['assets/icons/ic_instruction_surface.png'],
                    ),
                ],
                imageUrl: paths['assets/images/my_instruction_image.png'],
            ),
        ],
    ),
);
```

> **Note:** Make sure assets are declared in your `pubspec.yaml` under the `flutter.assets` section.

</details>

<details>
<summary><code><b>React Native</b></code> <small><i>· click to expand</i></small></summary>

Use `require()` to import assets and `Image.resolveAssetSource()` to convert them to URIs that the native SDK can use:

```typescript
import { Image } from 'react-native';

// Import assets using require()
const iconSource = require('./assets/icons/ic_instruction_surface.png');
const imageSource = require('./assets/images/my_instruction_image.png');

// Helper to convert to URI
const getAssetUri = (source: any): string => {
    const resolved = Image.resolveAssetSource(source);
    return resolved?.uri || '';
};

const theme: OnBoardIdTheme = {
    instructions: [
        {
            idType: 'DRIVERS_LICENSE',
            pictureType: 'FRONT',
            title: 'Scan Your License',
            steps: [
                {
                    text: 'Place on a flat surface',
                    iconUrl: getAssetUri(iconSource),
                },
            ],
            imageUrl: getAssetUri(imageSource),
        },
    ],
};

await netkiSDK.applyTheme(JSON.stringify(theme));
```

> **Note:** Assets must be placed in your project's assets folder and will be bundled automatically by Metro.

</details>

---

| Platform | Local Resource Format | Example |
|----------|----------------------|---------|
| Android | `android.resource://{packageName}/{R.drawable.name}` | `"android.resource://com.example.app/${R.drawable.ic_surface}"` |
| iOS | Asset catalog name (no extension) | `"ic_surface"` |
| Flutter | Resolved asset path via `resolveAssetPath()` | `await NetkiSdk.instance.resolveAssetPath('assets/icons/ic.png')` |
| React Native | URI from `Image.resolveAssetSource()` | `Image.resolveAssetSource(require('./icon.png')).uri` |
| All | Remote URL | `"https://example.com/icon.png"` |

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

---

<details>
<summary><code><b>Android</b></code> Kotlin <small><i>· click to expand</i></small></summary>

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

</details>

<details>
<summary><code><b>iOS</b></code> Swift <small><i>· click to expand</i></small></summary>

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

</details>

<details>
<summary><code><b>Flutter</b></code> Dart <small><i>· click to expand</i></small></summary>

```dart
// Resolve Flutter assets to file paths first
final paths = await NetkiSdk.instance.resolveAssetPaths([
    'assets/icons/ic_instruction_surface.png',
    'assets/icons/ic_instruction_corners.png',
    'assets/icons/ic_instruction_light.png',
    'assets/icons/ic_instruction_warning.png',
    'assets/icons/ic_instruction_face.png',
    'assets/images/dl_front_guide.png',
]);

await NetkiSdk.instance.applyTheme(
    theme: OnBoardIdTheme(
        buttons: ButtonColors(
            primaryBackground: '#8C5A3C',   // Medium brown
            primaryText: '#FFF8F0',         // Cream
            secondaryBackground: '#FFF8F0', // Cream
            secondaryText: '#4B2E2B',       // Dark brown
            cornerRadiusDp: 12,
        ),
        content: ContentColors(
            background: '#FFF8F0',   // Cream
            surface: '#F5EBE0',      // Warm light tan (cards)
            border: '#C08552',       // Caramel/tan
            titleText: '#4B2E2B',    // Dark brown
            subtitleText: '#8C5A3C', // Medium brown
            bodyText: '#4B2E2B',     // Dark brown
        ),
        instructions: [
            InstructionContent(
                idType: IdType.driversLicense,
                pictureType: PictureType.front,
                title: "Let's Verify Your Identity",
                subtitle: 'Driver\'s License - Front',
                steps: [
                    InstructionItem(
                        text: 'Place your ID on a dark, flat surface',
                        iconUrl: paths['assets/icons/ic_instruction_surface.png'],
                    ),
                    InstructionItem(
                        text: 'Make sure all four corners are visible',
                        iconUrl: paths['assets/icons/ic_instruction_corners.png'],
                    ),
                    InstructionItem(
                        text: 'Avoid glare and shadows on the document',
                        iconUrl: paths['assets/icons/ic_instruction_light.png'],
                    ),
                    InstructionItem(
                        text: 'Do not cover any information with your fingers',
                        iconUrl: paths['assets/icons/ic_instruction_warning.png'],
                        isWarning: true,
                    ),
                ],
                imageUrl: paths['assets/images/dl_front_guide.png'],
            ),
            InstructionContent(
                idType: IdType.biometrics,
                pictureType: PictureType.selfie,
                title: 'Almost Done!',
                subtitle: 'Take a Selfie',
                steps: [
                    InstructionItem(
                        text: 'Remove glasses, hats, and masks',
                        iconUrl: paths['assets/icons/ic_instruction_face.png'],
                    ),
                    InstructionItem(
                        text: 'Find good lighting - face a window if possible',
                        iconUrl: paths['assets/icons/ic_instruction_light.png'],
                    ),
                    InstructionItem(
                        text: 'Look straight at the camera',
                        iconUrl: paths['assets/icons/ic_instruction_face.png'],
                    ),
                ],
            ),
        ],
    ),
);
```

</details>

<details>
<summary><code><b>React Native</b></code> TypeScript <small><i>· click to expand</i></small></summary>

```typescript
import { Image } from 'react-native';
import netkiSDK from '@netki/netki-mobilesdk';
import type { OnBoardIdTheme } from '@netki/netki-mobilesdk';

// Import assets using require()
const icons = {
    surface: require('./assets/icons/ic_instruction_surface.png'),
    corners: require('./assets/icons/ic_instruction_corners.png'),
    light: require('./assets/icons/ic_instruction_light.png'),
    warning: require('./assets/icons/ic_instruction_warning.png'),
    face: require('./assets/icons/ic_instruction_face.png'),
    dlFrontImage: require('./assets/images/dl_front_guide.png'),
};

// Helper to convert to URI
const getAssetUri = (source: any): string => {
    const resolved = Image.resolveAssetSource(source);
    return resolved?.uri || '';
};

const theme: OnBoardIdTheme = {
    buttons: {
        primaryBackground: '#8C5A3C',   // Medium brown
        primaryText: '#FFF8F0',         // Cream
        secondaryBackground: '#FFF8F0', // Cream
        secondaryText: '#4B2E2B',       // Dark brown
        cornerRadiusDp: 12,
    },
    content: {
        background: '#FFF8F0',   // Cream
        surface: '#F5EBE0',      // Warm light tan (cards)
        border: '#C08552',       // Caramel/tan
        titleText: '#4B2E2B',    // Dark brown
        subtitleText: '#8C5A3C', // Medium brown
        bodyText: '#4B2E2B',     // Dark brown
    },
    instructions: [
        {
            idType: 'DRIVERS_LICENSE',
            pictureType: 'FRONT',
            title: "Let's Verify Your Identity",
            subtitle: "Driver's License - Front",
            steps: [
                {
                    text: 'Place your ID on a dark, flat surface',
                    iconUrl: getAssetUri(icons.surface),
                },
                {
                    text: 'Make sure all four corners are visible',
                    iconUrl: getAssetUri(icons.corners),
                },
                {
                    text: 'Avoid glare and shadows on the document',
                    iconUrl: getAssetUri(icons.light),
                },
                {
                    text: 'Do not cover any information with your fingers',
                    iconUrl: getAssetUri(icons.warning),
                    isWarning: true,
                },
            ],
            imageUrl: getAssetUri(icons.dlFrontImage),
        },
        {
            idType: 'BIOMETRICS',
            pictureType: 'SELFIE',
            title: 'Almost Done!',
            subtitle: 'Take a Selfie',
            steps: [
                {
                    text: 'Remove glasses, hats, and masks',
                    iconUrl: getAssetUri(icons.face),
                },
                {
                    text: 'Find good lighting - face a window if possible',
                    iconUrl: getAssetUri(icons.light),
                },
                {
                    text: 'Look straight at the camera',
                    iconUrl: getAssetUri(icons.face),
                },
            ],
        },
    ],
};

await netkiSDK.applyTheme(JSON.stringify(theme));
```

</details>

---

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

### Entry Point

The method for applying themes:

| Platform | Entry Point |
|----------|-------------|
| Android | `OnBoardIdUiV2.applyTheme(theme)` |
| iOS | `OnBoardIdUiV2.applyTheme(theme)` |
| Flutter | `await NetkiSdk.instance.applyTheme(theme: theme)` |
| React Native | `await netkiSDK.applyTheme(JSON.stringify(theme))` |

**Additional Methods (Android/iOS only):**

| Method | Description |
|--------|-------------|
| `enable()` | Enable V2 UI (only needed if you previously switched to V1) |

**Flutter Asset Resolution:**

| Method | Description |
|--------|-------------|
| `resolveAssetPath(assetPath)` | Resolve a single Flutter asset to a file path |
| `resolveAssetPaths(assetPaths)` | Resolve multiple Flutter assets to file paths (returns Map) |

**React Native Asset Resolution:**

| Method | Description |
|--------|-------------|
| `Image.resolveAssetSource(require('./path'))` | Resolve a bundled asset to a source object with `uri` property |

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

| Value | Android | iOS | Flutter | React Native | Description |
|-------|---------|-----|---------|--------------|-------------|
| Driver's License | `IdType.DRIVERS_LICENSE` | `.DRIVERS_LICENSE` | `IdType.driversLicense` | `'DRIVERS_LICENSE'` | Driver's license |
| Passport | `IdType.PASSPORT` | `.PASSPORT` | `IdType.passport` | `'PASSPORT'` | Passport |
| Government ID | `IdType.GOVERNMENT_ID` | `.GOVERNMENT_ID` | `IdType.governmentId` | `'GOVERNMENT_ID'` | National ID card |
| Biometrics | `IdType.BIOMETRICS` | `.BIOMETRICS` | `IdType.biometrics` | `'BIOMETRICS'` | Selfie/liveness |

### PictureType Values

| Value | Android | iOS | Flutter | React Native | Description |
|-------|---------|-----|---------|--------------|-------------|
| Front | `PictureType.FRONT` | `.FRONT` | `PictureType.front` | `'FRONT'` | Document front |
| Back | `PictureType.BACK` | `.BACK` | `PictureType.back` | `'BACK'` | Document back |
| Selfie | `PictureType.SELFIE` | `.SELFIE` | `PictureType.selfie` | `'SELFIE'` | Selfie capture |
| Liveness | `PictureType.LIVENESS` | `.LIVENESS` | `PictureType.liveness` | `'LIVENESS'` | Liveness check |
| E-Passport | `PictureType.EPASSPORT` | `.EPASSPORT` | `PictureType.epassport` | `'EPASSPORT'` | NFC passport |
