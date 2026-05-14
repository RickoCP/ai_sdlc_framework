# Signal Design System — Complete Reference

## Foundation

### index.css

```css
@import './colors.css';
@import './typography.css';
@import './spacing.css';
```

### colors.css

```css
/* Signal Design System - Color Tokens */
/* Source: signal-ui-react color.scss */

:root {
  /* Primary (Red - Brand) */
  --signal-color-primary-0: #fff5f6;
  --signal-color-primary-1: #ffebee;
  --signal-color-primary-2: #ffccd3;
  --signal-color-primary-3: #ff99a8;
  --signal-color-primary-4: #ff3351;
  --signal-color-primary-5: #ff0025;
  --signal-color-primary-6: #e50021;
  --signal-color-primary-7: #c2001c;
  --signal-color-primary-8: #940015;
  --signal-color-primary-9: #66000f;

  /* Secondary (Navy) */
  --signal-color-secondary-0: #fcfdfd;
  --signal-color-secondary-1: #f9f9fa;
  --signal-color-secondary-2: #f1f1f4;
  --signal-color-secondary-3: #dfe2e7;
  --signal-color-secondary-4: #b3bac6;
  --signal-color-secondary-5: #8c99ac;
  --signal-color-secondary-6: #334867;
  --signal-color-secondary-7: #001a41;
  --signal-color-secondary-8: #000e2e;
  --signal-color-secondary-9: #00071f;

  /* Valid (Green) */
  --signal-color-valid-0: #f2fdf4;
  --signal-color-valid-1: #edfcf0;
  --signal-color-valid-2: #9aeeab;
  --signal-color-valid-3: #0fd277;
  --signal-color-valid-4: #00b86b;
  --signal-color-valid-5: #008e53;
  --signal-color-valid-6: #007545;
  --signal-color-valid-7: #005c36;
  --signal-color-valid-8: #004227;
  --signal-color-valid-9: #00331e;

  /* Info (Blue) */
  --signal-color-info-0: #f7fcff;
  --signal-color-info-1: #e9f6ff;
  --signal-color-info-2: #94cff6;
  --signal-color-info-3: #5ca8e6;
  --signal-color-info-4: #3381ce;
  --signal-color-info-5: #0050ae;
  --signal-color-info-6: #003d95;
  --signal-color-info-7: #002d7d;
  --signal-color-info-8: #002064;
  --signal-color-info-9: #001653;

  /* Warning (Orange) */
  --signal-color-warning-0: #fefbf1;
  --signal-color-warning-1: #fef3d4;
  --signal-color-warning-2: #fee5aa;
  --signal-color-warning-3: #fed27f;
  --signal-color-warning-4: #fdc05f;
  --signal-color-warning-5: #fda22b;
  --signal-color-warning-6: #d9801f;
  --signal-color-warning-7: #b66215;
  --signal-color-warning-8: #92470d;
  --signal-color-warning-9: #793408;

  /* Error (Red-Pink) */
  --signal-color-error-0: #feefeb;
  --signal-color-error-1: #fdddd4;
  --signal-color-error-2: #fbb5a9;
  --signal-color-error-3: #f4837d;
  --signal-color-error-4: #e95b61;
  --signal-color-error-5: #db2941;
  --signal-color-error-6: #bc1d42;
  --signal-color-error-7: #9d1440;
  --signal-color-error-8: #7f0d3c;
  --signal-color-error-9: #690739;

  /* Neutral (Gray) */
  --signal-color-neutral-0: #ffffff;
  --signal-color-neutral-1: #fafafb;
  --signal-color-neutral-2: #e9e8ed;
  --signal-color-neutral-3: #cccfd3;
  --signal-color-neutral-4: #99a0a7;
  --signal-color-neutral-5: #66707a;
  --signal-color-neutral-6: #4e5764;

  /* Semantic aliases (for backward compat) */
  --color-primary: var(--signal-color-primary-5);
  --color-secondary: var(--signal-color-secondary-7);

  --text-primary: var(--signal-color-secondary-7);
  --text-secondary: var(--signal-color-neutral-6);
  --text-white: var(--signal-color-neutral-0);
  --text-link: var(--signal-color-info-5);
  --text-valid: var(--signal-color-valid-5);
  --text-warning: var(--signal-color-warning-5);
  --text-error: var(--signal-color-error-6);

  --bg-primary: var(--signal-color-primary-5);
  --bg-secondary: var(--signal-color-secondary-7);
  --bg-disable: var(--signal-color-secondary-2);
  --bg-page: var(--signal-color-neutral-0);
  --bg-card: var(--signal-color-neutral-0);
  --border-color: var(--signal-color-neutral-2);

  --gradient-primary: linear-gradient(90deg, #ed0226 0%, #b90024 100%);
  --gradient-secondary: linear-gradient(to right, #ed0226, #fda22b);
  --gradient-tertiary: linear-gradient(90deg, #001a41 0%, #0e336c 100%);
}

/* Dark Mode */
:root.dark {
  --text-primary: #e8e8e8;
  --text-secondary: #a0a0a0;
  --text-white: #ffffff;
  --text-link: #94cff6;
  --text-valid: #9aeeab;
  --text-warning: #fdc05f;
  --text-error: #fbb5a9;

  --bg-primary: var(--signal-color-primary-5);
  --bg-secondary: var(--signal-color-secondary-8);
  --bg-disable: #2d2d44;
  --bg-page: #1a1a2e;
  --bg-card: #16213e;
  --border-color: #2d2d44;
}
```

### typography.css

```css
/* Signal Design System - Typography Tokens */
/* Source: signal-ui-react typography.scss & fonts.scss */

@font-face {
  font-family: 'Telkomsel Batik Sans';
  font-style: normal;
  font-weight: 400;
  src: url('/fonts/TelkomselBatikSans-Regular.otf') format('opentype');
}

@font-face {
  font-family: 'Telkomsel Batik Sans';
  font-style: normal;
  font-weight: 700;
  src: url('/fonts/TelkomselBatikSans-Bold.otf') format('opentype');
}

@font-face {
  font-family: 'Poppins';
  font-style: normal;
  font-weight: 400;
  src: url('/fonts/Poppins-Regular.ttf') format('truetype');
}

@font-face {
  font-family: 'Poppins';
  font-style: normal;
  font-weight: 600;
  src: url('/fonts/Poppins-SemiBold.ttf') format('truetype');
}

@font-face {
  font-family: 'Poppins';
  font-style: normal;
  font-weight: 700;
  src: url('/fonts/Poppins-Bold.ttf') format('truetype');
}

:root {
  /* Font Families */
  --font-family-heading: 'Telkomsel Batik Sans', sans-serif;
  --font-family-body: 'Poppins', sans-serif;
  --font-family-main: 'Poppins', sans-serif;

  /* Heading Sizes (Telkomsel Batik Sans - Bold) */
  --font-size-heading-1: 44px;
  --font-size-heading-2: 32px;
  --font-size-heading-3: 28px;
  --font-size-heading-4: 24px;
  --font-size-heading-5: 18px;
  --font-size-heading-6: 16px;

  /* Line Heights - Headings */
  --line-height-heading-1: 56px;
  --line-height-heading-2: 48px;
  --line-height-heading-3: 40px;
  --line-height-heading-4: 40px;
  --line-height-heading-5: 24px;
  --line-height-heading-6: 24px;

  /* Body Sizes (Poppins) */
  --font-size-title: 16px;
  --font-size-body-1: 14px;
  --font-size-body-2: 12px;
  --font-size-label: 10px;

  /* Line Heights - Body */
  --line-height-title: 24px;
  --line-height-body-1: 20px;
  --line-height-body-2: 20px;
  --line-height-label: 16px;

  /* Legacy aliases */
  --font-size-caption: 12px;
  --line-height-heading: 1.25;
  --line-height-body: 1.5;
}
```

### spacing.css

```css
/* Signal Design System - Spacing, Radius, Shadow Tokens */
/* Source: signal-ui-react border.scss */

:root {
  /* Spacing */
  --space-1: 4px;
  --space-2: 8px;
  --space-3: 12px;
  --space-4: 16px;
  --space-5: 24px;
  --space-6: 32px;
  --space-7: 48px;
  --space-8: 64px;

  /* Border Radius */
  --radius-sm: 4px;
  --radius-md: 8px;
  --radius-lg: 12px;
  --radius-xl: 20px;
  --radius-full: 9999px;

  /* Box Shadows */
  --shadow-none: none;
  --shadow-sm: 0px 4px 4px 0px rgba(158, 158, 158, 0.25);
  --shadow-md: 4px 0px 4px 0px rgba(158, 158, 158, 0.2);
  --shadow-lg: 0 8px 24px rgba(0, 0, 0, 0.16);

  /* Border Sizes */
  --border-1: 1px solid transparent;
  --border-2: 2px solid transparent;
  --border-3: 3px solid transparent;
}
```

## Components

### index.ts

```typescript
export { Button } from './Button';
export type { ButtonProps } from './Button';

export { TextField } from './TextField';
export type { TextFieldProps } from './TextField';

export { Badges } from './Badges';
export type { BadgesProps } from './Badges';

export { Chips } from './Chips';
export type { ChipsProps, ChipItem } from './Chips';

export { Callout } from './Callout';
export type { CalloutProps } from './Callout';

export { Modal } from './Modal';
export type { ModalProps } from './Modal';

export { Snackbar } from './Snackbar';
export type { SnackbarProps } from './Snackbar';

export { Icon } from './Icon';
export type { IconProps, IconSize } from './Icon';

export { Text } from './Text';
export type { TextProps } from './Text';

export { Label } from './Label';
export type { LabelProps } from './Label';

export { Checkbox } from './Checkbox';
export type { CheckboxProps, CheckboxItem } from './Checkbox';

export { CheckboxGroup } from './CheckboxGroup';
export type { CheckboxGroupProps } from './CheckboxGroup';

export { Radio } from './Radio';
export type { RadioProps, RadioItem } from './Radio';

export { Switch } from './Switch';
export type { SwitchProps } from './Switch';

export { Tab } from './Tab';
export type { TabProps, TabItem } from './Tab';

export { TextArea } from './TextArea';
export type { TextAreaProps } from './TextArea';

export { OTP } from './OTP';
export type { OTPProps } from './OTP';

export { Breadcrumb } from './Breadcrumb';
export type { BreadcrumbProps, BreadcrumbItem } from './Breadcrumb';

export { BottomSheet } from './BottomSheet';
export type { BottomSheetProps } from './BottomSheet';
```

### Button

#### Button.tsx

```tsx
'use client';

import { CSSProperties, ReactNode } from 'react';
import clsx from 'clsx';
import './Button.css';

export interface ButtonProps {
  children: ReactNode;
  disabled?: boolean;
  full?: boolean;
  color?: 'primary' | 'secondary' | 'valid' | 'warning' | 'info';
  size?: 'small' | 'medium' | 'large';
  variant?: 'primary' | 'secondary' | 'tertiary';
  className?: string;
  justify?: 'center' | 'space-between';
  style?: CSSProperties;
  transparent?: boolean;
  onClick?: () => void;
}

export function Button({
  children,
  disabled = false,
  full = false,
  color = 'primary',
  size = 'medium',
  variant = 'primary',
  className,
  justify = 'center',
  style,
  transparent = false,
  onClick,
}: ButtonProps) {
  const buttonClass = clsx(
    'signal-button-root',
    `color-${color}`,
    `size-${size}`,
    `button-variant-${variant}`,
    {
      full,
      disabled,
      transparent,
    },
    className,
  );

  const bodyClass = clsx('signal-button-body', `justify-${justify}`);

  return (
    <button
      style={style}
      disabled={disabled}
      className={buttonClass}
      onClick={onClick}
    >
      <div className={bodyClass}>{children}</div>
    </button>
  );
}
```

#### Button.css

```css
.signal-button-root {
  border: 2px solid transparent;
  font-family: var(--font-family-body);
  border-radius: var(--radius-full);
  padding: 0 18px;
  cursor: pointer;
  transition: opacity 0.2s, transform 0.1s;
  line-height: 1;
}

.signal-button-root:hover:not(.disabled) {
  opacity: 0.9;
}

.signal-button-root:active:not(.disabled) {
  transform: scale(0.98);
}

/* Primary Variant */
.signal-button-root.button-variant-primary {
  color: var(--signal-color-neutral-0);
}
.signal-button-root.button-variant-primary.color-primary {
  background-color: var(--signal-color-primary-5);
}
.signal-button-root.button-variant-primary.color-secondary {
  background-color: var(--signal-color-secondary-7);
}
.signal-button-root.button-variant-primary.color-valid {
  background-color: var(--signal-color-valid-5);
}
.signal-button-root.button-variant-primary.color-warning {
  background-color: var(--signal-color-warning-5);
}
.signal-button-root.button-variant-primary.color-info {
  background-color: var(--signal-color-info-5);
}
.signal-button-root.button-variant-primary.disabled {
  color: var(--signal-color-secondary-4);
  cursor: not-allowed;
  background-color: var(--signal-color-neutral-2);
}

/* Secondary Variant (Outlined) */
.signal-button-root.button-variant-secondary {
  background-color: transparent;
}
.signal-button-root.button-variant-secondary.color-primary {
  color: var(--signal-color-primary-5);
  border-color: var(--signal-color-primary-5);
}
.signal-button-root.button-variant-secondary.color-secondary {
  color: var(--signal-color-secondary-7);
  border-color: var(--signal-color-secondary-7);
}
.signal-button-root.button-variant-secondary.color-valid {
  color: var(--signal-color-valid-5);
  border-color: var(--signal-color-valid-5);
}
.signal-button-root.button-variant-secondary.color-info {
  color: var(--signal-color-info-5);
  border-color: var(--signal-color-info-5);
}
.signal-button-root.button-variant-secondary.color-warning {
  color: var(--signal-color-warning-5);
  border-color: var(--signal-color-warning-5);
}
.signal-button-root.button-variant-secondary.disabled {
  color: var(--signal-color-secondary-4);
  cursor: not-allowed;
  border-color: var(--signal-color-secondary-4);
}

/* Tertiary Variant (Ghost) */
.signal-button-root.button-variant-tertiary {
  background-color: var(--signal-color-neutral-0);
  border-color: transparent;
}
.signal-button-root.button-variant-tertiary.color-primary {
  color: var(--signal-color-primary-5);
}
.signal-button-root.button-variant-tertiary.color-secondary {
  color: var(--signal-color-secondary-7);
}
.signal-button-root.button-variant-tertiary.color-valid {
  color: var(--signal-color-valid-5);
}
.signal-button-root.button-variant-tertiary.color-info {
  color: var(--signal-color-info-5);
}
.signal-button-root.button-variant-tertiary.color-warning {
  color: var(--signal-color-warning-5);
}
.signal-button-root.button-variant-tertiary.disabled {
  color: var(--signal-color-secondary-4);
  cursor: not-allowed;
}

/* Sizes */
.signal-button-root.size-small {
  font-size: 14px;
  padding: 4px 12px;
}
.signal-button-root.size-medium {
  font-size: 16px;
  padding: 8px 16px;
}
.signal-button-root.size-large {
  font-size: 20px;
  padding: 12px 20px;
}

/* Full Width */
.signal-button-root.full {
  width: 100%;
  height: 42px;
  padding: 0 16px;
}

/* Transparent */
.signal-button-root.transparent {
  background-color: transparent;
}

/* Button Body */
.signal-button-body {
  display: flex;
  align-items: center;
  text-align: center;
  gap: 8px;
}
.signal-button-body.justify-center {
  justify-content: center;
}
.signal-button-body.justify-space-between {
  justify-content: space-between;
}
```

### TextField

#### TextField.tsx

```tsx
'use client';

import { ChangeEvent } from 'react';
import clsx from 'clsx';
import './TextField.css';

export interface TextFieldProps {
  name?: string;
  placeholder?: string;
  variant?: 'primary' | 'secondary';
  labelColor?: string;
  supportText?: string;
  errorText?: string;
  successText?: string;
  disabled?: boolean;
  required?: boolean;
  value?: string;
  type?: string;
  onChange?: (e: ChangeEvent<HTMLInputElement>) => void;
  className?: string;
}

export function TextField({
  name,
  placeholder,
  variant = 'primary',
  labelColor,
  supportText,
  errorText,
  successText,
  disabled,
  required,
  value,
  type = 'text',
  onChange,
  className,
}: TextFieldProps) {
  const inputClass = clsx('signal-textfield-input', `input-${variant}`, {
    'input-error': !!errorText,
    'input-success': !!successText,
    'input-disabled': disabled,
  });

  return (
    <div className={clsx('signal-textfield-root', className)}>
      {name && (
        <label className="signal-textfield-label" style={{ color: labelColor }}>
          {name}
          {required && <span className="signal-textfield-required">*</span>}
        </label>
      )}
      <input
        type={type}
        value={value ?? ''}
        onChange={onChange ?? (() => {})}
        className={inputClass}
        placeholder={placeholder}
        disabled={disabled}
        readOnly={!onChange}
        aria-invalid={!!errorText}
        aria-describedby={errorText ? 'error-msg' : supportText ? 'support-msg' : undefined}
      />
      {errorText && (
        <p id="error-msg" className="signal-textfield-error">{errorText}</p>
      )}
      {supportText && !errorText && (
        <p id="support-msg" className="signal-textfield-support">{supportText}</p>
      )}
      {successText && !errorText && (
        <p className="signal-textfield-success">{successText}</p>
      )}
    </div>
  );
}
```

#### TextField.css

```css
.signal-textfield-root {
  display: flex;
  flex-direction: column;
  gap: 4px;
  width: 100%;
}

.signal-textfield-label {
  font-family: var(--font-family-body);
  font-size: var(--font-size-body-2);
  font-weight: 600;
  color: var(--signal-color-secondary-7);
  line-height: var(--line-height-body-1);
}

.signal-textfield-required {
  color: var(--signal-color-error-5);
  margin-left: 2px;
}

.signal-textfield-input {
  font-family: var(--font-family-body);
  font-size: var(--font-size-body-1);
  padding: 10px 14px;
  border: 1px solid var(--signal-color-neutral-3);
  border-radius: var(--radius-md);
  outline: none;
  transition: border-color 0.2s, box-shadow 0.2s;
  background: var(--signal-color-neutral-0);
  color: var(--signal-color-secondary-7);
  width: 100%;
}

.signal-textfield-input::placeholder {
  color: var(--signal-color-secondary-4);
}

.signal-textfield-input:focus {
  border-color: var(--signal-color-primary-5);
  box-shadow: 0 0 0 2px var(--signal-color-primary-1);
}

.signal-textfield-input.input-secondary:focus {
  border-color: var(--signal-color-secondary-7);
  box-shadow: 0 0 0 2px var(--signal-color-secondary-2);
}

.signal-textfield-input.input-error {
  border-color: var(--signal-color-error-5);
}

.signal-textfield-input.input-error:focus {
  box-shadow: 0 0 0 2px var(--signal-color-error-1);
}

.signal-textfield-input.input-success {
  border-color: var(--signal-color-valid-5);
}

.signal-textfield-input.input-disabled {
  background: var(--signal-color-neutral-1);
  color: var(--signal-color-secondary-4);
  cursor: not-allowed;
}

.signal-textfield-error {
  font-size: var(--font-size-body-2);
  color: var(--signal-color-error-5);
  margin: 0;
}

.signal-textfield-support {
  font-size: var(--font-size-body-2);
  color: var(--signal-color-neutral-5);
  margin: 0;
}

.signal-textfield-success {
  font-size: var(--font-size-body-2);
  color: var(--signal-color-valid-5);
  margin: 0;
}
```

### Badges

#### Badges.tsx

```tsx
'use client';

import { ReactNode } from 'react';
import clsx from 'clsx';
import './Badges.css';

export interface BadgesProps {
  children?: ReactNode;
  badgeContent?: string;
  color?: string;
  textColor?: string;
  vertical?: 'top' | 'bottom';
  horizontal?: 'left' | 'right';
  className?: string;
}

export function Badges({
  children,
  badgeContent,
  color = 'primary-5',
  textColor = 'neutral-0',
  vertical = 'top',
  horizontal = 'right',
  className,
}: BadgesProps) {
  const containerClass = clsx(
    'signal-badges-container',
    `vertical-${vertical}`,
    `horizontal-${horizontal}`,
    {
      short: badgeContent && badgeContent.length <= 2,
      long: badgeContent && badgeContent.length > 2,
    },
    className,
  );

  return (
    <div className="signal-badges-root">
      <div
        className={containerClass}
        style={{
          backgroundColor: `var(--signal-color-${color})`,
          color: `var(--signal-color-${textColor})`,
        }}
      >
        {badgeContent}
      </div>
      {children}
    </div>
  );
}
```

#### Badges.css

```css
.signal-badges-root {
  position: relative;
  display: inline-flex;
}

.signal-badges-container {
  position: absolute;
  font-family: var(--font-family-body);
  font-size: 10px;
  font-weight: 700;
  line-height: 1;
  border-radius: var(--radius-full);
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 1;
}

.signal-badges-container.short {
  min-width: 18px;
  height: 18px;
  padding: 2px 4px;
}

.signal-badges-container.long {
  min-width: 24px;
  height: 18px;
  padding: 2px 6px;
}

.signal-badges-container.vertical-top {
  top: -6px;
}

.signal-badges-container.vertical-bottom {
  bottom: -6px;
}

.signal-badges-container.horizontal-right {
  right: -6px;
}

.signal-badges-container.horizontal-left {
  left: -6px;
}
```

### Chips

#### Chips.tsx

```tsx
'use client';

import { useState } from 'react';
import clsx from 'clsx';
import './Chips.css';

export interface ChipItem {
  id: string;
  name: string;
  isDisabled?: boolean;
}

export interface ChipsProps {
  keywords: ChipItem[];
  onChipSelected?: (chip: ChipItem) => void;
  onSelectedChipsChange?: (chips: ChipItem[]) => void;
  className?: string;
}

export function Chips({
  keywords,
  onChipSelected,
  onSelectedChipsChange,
  className,
}: ChipsProps) {
  const [selectedChips, setSelectedChips] = useState<ChipItem[]>([]);

  const handleChipClick = (keyword: ChipItem) => {
    if (keyword.isDisabled) return;

    let updated: ChipItem[];
    if (selectedChips.some((chip) => chip.id === keyword.id)) {
      updated = selectedChips.filter((chip) => chip.id !== keyword.id);
    } else {
      updated = [...selectedChips, keyword];
    }

    setSelectedChips(updated);
    onChipSelected?.(keyword);
    onSelectedChipsChange?.(updated);
  };

  return (
    <div className={clsx('signal-chips-root', className)}>
      {keywords.map((keyword) => {
        const chipClass = clsx('signal-chip', {
          selected: selectedChips.some((chip) => chip.id === keyword.id),
          disabled: keyword.isDisabled,
        });

        return (
          <button
            key={keyword.id}
            className={chipClass}
            onClick={() => handleChipClick(keyword)}
            disabled={keyword.isDisabled}
          >
            <span>{keyword.name}</span>
          </button>
        );
      })}
    </div>
  );
}
```

#### Chips.css

```css
.signal-chips-root {
  display: flex;
  flex-wrap: wrap;
  gap: 8px;
}

.signal-chip {
  font-family: var(--font-family-body);
  font-size: var(--font-size-body-2);
  padding: 6px 14px;
  border-radius: var(--radius-full);
  border: 1px solid var(--signal-color-neutral-3);
  background: var(--signal-color-neutral-0);
  color: var(--signal-color-secondary-7);
  cursor: pointer;
  transition: all 0.2s;
  display: inline-flex;
  align-items: center;
  gap: 6px;
}

.signal-chip:hover:not(.disabled) {
  border-color: var(--signal-color-primary-5);
  color: var(--signal-color-primary-5);
}

.signal-chip.selected {
  background: var(--signal-color-primary-5);
  border-color: var(--signal-color-primary-5);
  color: var(--signal-color-neutral-0);
}

.signal-chip.disabled {
  background: var(--signal-color-neutral-1);
  color: var(--signal-color-secondary-4);
  cursor: not-allowed;
  border-color: var(--signal-color-neutral-2);
}
```

### Callout

#### Callout.tsx

```tsx
'use client';

import clsx from 'clsx';
import './Callout.css';

export interface CalloutProps {
  type?: 'info' | 'warning' | 'error' | 'success';
  title: string;
  description?: string;
  actionText?: string;
  onAction?: () => void;
  className?: string;
}

export function Callout({
  type = 'info',
  title,
  description,
  actionText,
  onAction,
  className,
}: CalloutProps) {
  const icons: Record<string, string> = {
    info: 'ℹ️',
    warning: '⚠️',
    error: '❌',
    success: '✅',
  };

  return (
    <div className={clsx('signal-callout-root', `callout-${type}`, className)}>
      <div className="signal-callout-icon">{icons[type]}</div>
      <div className="signal-callout-content">
        <p className="signal-callout-title">{title}</p>
        {description && <p className="signal-callout-desc">{description}</p>}
        {actionText && (
          <button className="signal-callout-action" onClick={onAction}>
            {actionText}
          </button>
        )}
      </div>
    </div>
  );
}
```

#### Callout.css

```css
.signal-callout-root {
  display: flex;
  gap: 12px;
  padding: 14px 16px;
  border-radius: var(--radius-md);
  border-left: 4px solid transparent;
  font-family: var(--font-family-body);
}

.signal-callout-root.callout-info {
  background: var(--signal-color-info-0);
  border-left-color: var(--signal-color-info-5);
}

.signal-callout-root.callout-warning {
  background: var(--signal-color-warning-0);
  border-left-color: var(--signal-color-warning-5);
}

.signal-callout-root.callout-error {
  background: var(--signal-color-error-0);
  border-left-color: var(--signal-color-error-5);
}

.signal-callout-root.callout-success {
  background: var(--signal-color-valid-0);
  border-left-color: var(--signal-color-valid-5);
}

.signal-callout-icon {
  font-size: 18px;
  flex-shrink: 0;
}

.signal-callout-content {
  display: flex;
  flex-direction: column;
  gap: 4px;
}

.signal-callout-title {
  font-size: var(--font-size-body-1);
  font-weight: 600;
  color: var(--signal-color-secondary-7);
  margin: 0;
}

.signal-callout-desc {
  font-size: var(--font-size-body-2);
  color: var(--signal-color-neutral-5);
  margin: 0;
}

.signal-callout-action {
  font-size: var(--font-size-body-2);
  font-weight: 600;
  color: var(--signal-color-info-5);
  background: none;
  border: none;
  cursor: pointer;
  padding: 0;
  text-align: left;
  margin-top: 4px;
}

.signal-callout-action:hover {
  text-decoration: underline;
}
```

### Modal

#### Modal.tsx

```tsx
'use client';

import { ReactNode, useEffect } from 'react';
import clsx from 'clsx';
import './Modal.css';

export interface ModalProps {
  open: boolean;
  title?: string;
  description?: string;
  children?: ReactNode;
  onClose?: () => void;
  className?: string;
}

export function Modal({
  open,
  title,
  description,
  children,
  onClose,
  className,
}: ModalProps) {
  useEffect(() => {
    if (open) {
      document.body.style.overflow = 'hidden';
    } else {
      document.body.style.overflow = '';
    }
    return () => {
      document.body.style.overflow = '';
    };
  }, [open]);

  if (!open) return null;

  return (
    <div className="signal-modal-overlay" onClick={onClose} role="dialog" aria-modal="true">
      <div
        className={clsx('signal-modal-content', className)}
        onClick={(e) => e.stopPropagation()}
      >
        {onClose && (
          <button className="signal-modal-close" onClick={onClose} aria-label="Close modal">
            ✕
          </button>
        )}
        {title && <h2 className="signal-modal-title">{title}</h2>}
        {description && <p className="signal-modal-desc">{description}</p>}
        {children && <div className="signal-modal-body">{children}</div>}
      </div>
    </div>
  );
}
```

#### Modal.css

```css
.signal-modal-overlay {
  position: fixed;
  inset: 0;
  background: rgba(0, 0, 0, 0.5);
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 1000;
  padding: 16px;
}

.signal-modal-content {
  background: var(--signal-color-neutral-0);
  border-radius: var(--radius-xl);
  padding: 24px;
  max-width: 480px;
  width: 100%;
  position: relative;
  box-shadow: var(--shadow-lg);
  font-family: var(--font-family-body);
}

.signal-modal-close {
  position: absolute;
  top: 16px;
  right: 16px;
  background: none;
  border: none;
  font-size: 18px;
  cursor: pointer;
  color: var(--signal-color-neutral-5);
  width: 28px;
  height: 28px;
  display: flex;
  align-items: center;
  justify-content: center;
  border-radius: var(--radius-full);
  transition: background 0.2s;
}

.signal-modal-close:hover {
  background: var(--signal-color-neutral-1);
}

.signal-modal-title {
  font-family: var(--font-family-heading);
  font-size: var(--font-size-heading-5);
  font-weight: 700;
  color: var(--signal-color-secondary-7);
  margin: 0 0 8px;
}

.signal-modal-desc {
  font-size: var(--font-size-body-1);
  color: var(--signal-color-neutral-5);
  margin: 0 0 20px;
}

.signal-modal-body {
  display: flex;
  gap: 12px;
  flex-wrap: wrap;
}
```

### Snackbar

#### Snackbar.tsx

```tsx
'use client';

import { useEffect, useState } from 'react';
import clsx from 'clsx';
import './Snackbar.css';

export interface SnackbarProps {
  message: string;
  type?: 'success' | 'error' | 'info' | 'warning';
  duration?: number;
  visible: boolean;
  onClose?: () => void;
  className?: string;
}

export function Snackbar({
  message,
  type = 'info',
  duration = 3000,
  visible,
  onClose,
  className,
}: SnackbarProps) {
  const [show, setShow] = useState(visible);

  useEffect(() => {
    setShow(visible);
    if (visible && duration > 0) {
      const timer = setTimeout(() => {
        setShow(false);
        onClose?.();
      }, duration);
      return () => clearTimeout(timer);
    }
  }, [visible, duration, onClose]);

  if (!show) return null;

  return (
    <div className={clsx('signal-snackbar', `snackbar-${type}`, className)} role="alert">
      <span className="signal-snackbar-message">{message}</span>
      <button className="signal-snackbar-close" onClick={() => { setShow(false); onClose?.(); }} aria-label="Dismiss">
        ✕
      </button>
    </div>
  );
}
```

#### Snackbar.css

```css
.signal-snackbar {
  position: fixed;
  bottom: 24px;
  left: 50%;
  transform: translateX(-50%);
  padding: 12px 20px;
  border-radius: var(--radius-md);
  font-family: var(--font-family-body);
  font-size: var(--font-size-body-1);
  display: flex;
  align-items: center;
  gap: 12px;
  z-index: 2000;
  box-shadow: var(--shadow-sm);
  animation: snackbar-in 0.3s ease;
  min-width: 280px;
  max-width: 480px;
}

@keyframes snackbar-in {
  from {
    opacity: 0;
    transform: translateX(-50%) translateY(20px);
  }
  to {
    opacity: 1;
    transform: translateX(-50%) translateY(0);
  }
}

.signal-snackbar.snackbar-success {
  background: var(--signal-color-valid-7);
  color: var(--signal-color-neutral-0);
}

.signal-snackbar.snackbar-error {
  background: var(--signal-color-error-6);
  color: var(--signal-color-neutral-0);
}

.signal-snackbar.snackbar-info {
  background: var(--signal-color-secondary-7);
  color: var(--signal-color-neutral-0);
}

.signal-snackbar.snackbar-warning {
  background: var(--signal-color-warning-5);
  color: var(--signal-color-secondary-7);
}

.signal-snackbar-message {
  flex: 1;
}

.signal-snackbar-close {
  background: none;
  border: none;
  color: inherit;
  cursor: pointer;
  font-size: 14px;
  opacity: 0.8;
  padding: 4px;
}

.signal-snackbar-close:hover {
  opacity: 1;
}
```

### Icon

#### Icon.tsx

```tsx
'use client';

import { CSSProperties } from 'react';
import clsx from 'clsx';
import './Icon.css';

export type IconSize = 'small' | 'medium' | 'large';

export interface IconProps {
  name: string;
  color?: string;
  size?: IconSize;
  style?: CSSProperties;
  className?: string;
}

export function Icon({ name, color, size = 'medium', style, className }: IconProps) {
  const iconClass = clsx(
    'signal-icon-root',
    `tsel-ico_${name}`,
    `size-${size}`,
    color && `signal-text-color-${color}`,
    className,
  );

  return <i className={iconClass} style={style} />;
}
```

#### Icon.css

```css
.signal-icon-root {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  font-style: normal;
}

.signal-icon-root.size-small {
  font-size: 16px;
  width: 16px;
  height: 16px;
}

.signal-icon-root.size-medium {
  font-size: 24px;
  width: 24px;
  height: 24px;
}

.signal-icon-root.size-large {
  font-size: 32px;
  width: 32px;
  height: 32px;
}
```

### Text

#### Text.tsx

```tsx
'use client';

import { MouseEventHandler, ReactNode } from 'react';
import clsx from 'clsx';
import './Text.css';

export interface TextProps {
  color?: string;
  fontWeight?: 'bold' | 'semibold' | 'regular';
  strikethrough?: boolean;
  variant?: 'h1' | 'h2' | 'h3' | 'h4' | 'h5' | 'h6' | 'title' | 'body1' | 'body2' | 'label';
  className?: string;
  children: ReactNode;
  onClick?: MouseEventHandler;
}

export function Text({
  color,
  fontWeight = 'regular',
  strikethrough = false,
  variant = 'body1',
  className,
  children,
  onClick,
}: TextProps) {
  const textClass = clsx(
    'signal-text',
    `signal-text-${variant}`,
    `signal-text-weight-${fontWeight}`,
    { 'signal-text-strikethrough': strikethrough },
    { 'signal-text-clickable': !!onClick },
    className,
  );

  return (
    <span className={textClass} onClick={onClick} style={color ? { color } : undefined}>
      {children}
    </span>
  );
}
```

#### Text.css

```css
.signal-text {
  font-family: var(--font-family-body);
}

/* Variants - Headings */
.signal-text-h1 { font-family: var(--font-family-heading); font-size: var(--font-size-heading-1); line-height: var(--line-height-heading-1); }
.signal-text-h2 { font-family: var(--font-family-heading); font-size: var(--font-size-heading-2); line-height: var(--line-height-heading-2); }
.signal-text-h3 { font-family: var(--font-family-heading); font-size: var(--font-size-heading-3); line-height: var(--line-height-heading-3); }
.signal-text-h4 { font-family: var(--font-family-heading); font-size: var(--font-size-heading-4); line-height: var(--line-height-heading-4); }
.signal-text-h5 { font-family: var(--font-family-heading); font-size: var(--font-size-heading-5); line-height: var(--line-height-heading-5); }
.signal-text-h6 { font-family: var(--font-family-heading); font-size: var(--font-size-heading-6); line-height: var(--line-height-heading-6); }

/* Variants - Body */
.signal-text-title { font-size: var(--font-size-title); line-height: var(--line-height-title); }
.signal-text-body1 { font-size: var(--font-size-body-1); line-height: var(--line-height-body-1); }
.signal-text-body2 { font-size: var(--font-size-body-2); line-height: var(--line-height-body-2); }
.signal-text-label { font-size: var(--font-size-label); line-height: var(--line-height-label); }

/* Weights */
.signal-text-weight-regular { font-weight: 400; }
.signal-text-weight-semibold { font-weight: 600; }
.signal-text-weight-bold { font-weight: 700; }

/* Modifiers */
.signal-text-strikethrough { text-decoration: line-through; }
.signal-text-clickable { cursor: pointer; }
```

### Label

#### Label.tsx

```tsx
'use client';

import { ReactNode } from 'react';
import clsx from 'clsx';
import './Label.css';

export interface LabelProps {
  color?: string;
  rounded?: '1' | '2' | '3' | '4';
  borderSize?: '0' | '1' | '2';
  roundedSize?: 'sm' | 'lg';
  borderColor?: string;
  children?: ReactNode;
  className?: string;
}

export function Label({
  color = 'var(--signal-color-primary-5)',
  rounded = '1',
  borderSize = '0',
  roundedSize = 'sm',
  borderColor,
  children,
  className,
}: LabelProps) {
  const labelClass = clsx(
    'signal-label-root',
    `border-size-${borderSize}`,
    `rounded-${roundedSize}-${rounded}`,
    className,
  );

  return (
    <div
      className={labelClass}
      style={{ backgroundColor: color, borderColor: borderColor || color }}
    >
      {children}
    </div>
  );
}
```

#### Label.css

```css
.signal-label-root {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  padding: 2px 8px;
  font-family: var(--font-family-body);
  font-size: var(--font-size-label);
  font-weight: 600;
  color: var(--signal-color-neutral-0);
  border: none;
}

.signal-label-root.border-size-1 { border: 1px solid; }
.signal-label-root.border-size-2 { border: 2px solid; }

.signal-label-root.rounded-sm-1 { border-radius: 4px; }
.signal-label-root.rounded-sm-2 { border-radius: 0 0 4px 0; }
.signal-label-root.rounded-sm-3 { border-radius: 4px 4px 4px 0; }
.signal-label-root.rounded-sm-4 { border-radius: 4px 0; }

.signal-label-root.rounded-lg-1 { border-radius: 20px; }
.signal-label-root.rounded-lg-2 { border-radius: 0 0 20px 0; }
.signal-label-root.rounded-lg-3 { border-radius: 20px 20px 20px 0; }
.signal-label-root.rounded-lg-4 { border-radius: 20px 0; }
```

### Checkbox

#### Checkbox.tsx

```tsx
'use client';

import { useState } from 'react';
import clsx from 'clsx';
import './Checkbox.css';

export interface CheckboxItem {
  label: string;
  value: string;
  checked: boolean;
}

export interface CheckboxProps {
  item: CheckboxItem;
  onChange: (item: CheckboxItem) => void;
  disabled?: boolean;
  className?: string;
}

export function Checkbox({ item, onChange, disabled = false, className }: CheckboxProps) {
  const [checked, setChecked] = useState(item.checked);

  const handleChange = () => {
    if (disabled) return;
    const newChecked = !checked;
    setChecked(newChecked);
    onChange({ ...item, checked: newChecked });
  };

  return (
    <label className={clsx('signal-checkbox-root', { disabled }, className)}>
      <input
        type="checkbox"
        checked={checked}
        onChange={handleChange}
        disabled={disabled}
        className="signal-checkbox-input"
      />
      <span className={clsx('signal-checkbox-box', { checked })} aria-hidden="true">
        {checked && (
          <svg width="12" height="10" viewBox="0 0 12 10" fill="none">
            <path d="M1 5L4.5 8.5L11 1.5" stroke="currentColor" strokeWidth="2" strokeLinecap="round" strokeLinejoin="round" />
          </svg>
        )}
      </span>
      <span className="signal-checkbox-label">{item.label}</span>
    </label>
  );
}
```

#### Checkbox.css

```css
.signal-checkbox-root {
  display: flex;
  align-items: center;
  gap: 8px;
  cursor: pointer;
  font-family: var(--font-family-body);
  font-size: var(--font-size-body-1);
  padding: 4px 0;
}

.signal-checkbox-root.disabled {
  cursor: not-allowed;
  opacity: 0.5;
}

.signal-checkbox-input {
  position: absolute;
  opacity: 0;
  width: 0;
  height: 0;
}

.signal-checkbox-box {
  width: 20px;
  height: 20px;
  border: 2px solid var(--signal-color-neutral-3);
  border-radius: var(--radius-sm);
  display: flex;
  align-items: center;
  justify-content: center;
  transition: all 0.2s;
  flex-shrink: 0;
  color: var(--signal-color-neutral-0);
}

.signal-checkbox-box.checked {
  background: var(--signal-color-info-5);
  border-color: var(--signal-color-info-5);
}

.signal-checkbox-label {
  color: var(--signal-color-secondary-7);
  user-select: none;
}
```

### CheckboxGroup

#### CheckboxGroup.tsx

```tsx
'use client';

import { Checkbox } from '../Checkbox';
import type { CheckboxItem } from '../Checkbox';
import './CheckboxGroup.css';

export interface CheckboxGroupProps {
  items: CheckboxItem[];
  onChange: (checkedItems: CheckboxItem[]) => void;
  disabled?: boolean;
  className?: string;
}

export function CheckboxGroup({ items, onChange, disabled, className }: CheckboxGroupProps) {
  const handleChange = (changedItem: CheckboxItem) => {
    const updated = items.map((item) =>
      item.value === changedItem.value ? changedItem : item,
    );
    onChange(updated.filter((item) => item.checked));
  };

  return (
    <div className={`signal-checkbox-group ${className || ''}`}>
      {items.map((item) => (
        <Checkbox
          key={item.value}
          item={item}
          onChange={handleChange}
          disabled={disabled}
        />
      ))}
    </div>
  );
}
```

#### CheckboxGroup.css

```css
.signal-checkbox-group {
  display: flex;
  flex-direction: column;
  gap: 4px;
}
```

### Radio

#### Radio.tsx

```tsx
'use client';

import { useState } from 'react';
import clsx from 'clsx';
import './Radio.css';

export interface RadioItem {
  label: string;
  value: string;
}

export interface RadioProps {
  items: RadioItem[];
  name: string;
  onChange: (selectedItem: RadioItem) => void;
  defaultValue?: string;
  disabled?: boolean;
  className?: string;
}

export function Radio({ items, name, onChange, defaultValue, disabled, className }: RadioProps) {
  const [selected, setSelected] = useState(defaultValue || '');

  const handleChange = (item: RadioItem) => {
    if (disabled) return;
    setSelected(item.value);
    onChange(item);
  };

  return (
    <div className={clsx('signal-radio-root', className)} role="radiogroup">
      {items.map((item) => (
        <label
          key={item.value}
          className={clsx('signal-radio-item', { disabled, selected: selected === item.value })}
        >
          <input
            type="radio"
            name={name}
            value={item.value}
            checked={selected === item.value}
            onChange={() => handleChange(item)}
            disabled={disabled}
            className="signal-radio-input"
          />
          <span className={clsx('signal-radio-circle', { checked: selected === item.value })} aria-hidden="true">
            {selected === item.value && <span className="signal-radio-dot" />}
          </span>
          <span className="signal-radio-label">{item.label}</span>
        </label>
      ))}
    </div>
  );
}
```

#### Radio.css

```css
.signal-radio-root {
  display: flex;
  flex-direction: column;
  gap: 4px;
}

.signal-radio-item {
  display: flex;
  align-items: center;
  gap: 8px;
  cursor: pointer;
  font-family: var(--font-family-body);
  font-size: var(--font-size-body-1);
  padding: 4px 0;
}

.signal-radio-item.disabled {
  cursor: not-allowed;
  opacity: 0.5;
}

.signal-radio-input {
  position: absolute;
  opacity: 0;
  width: 0;
  height: 0;
}

.signal-radio-circle {
  width: 20px;
  height: 20px;
  border: 2px solid var(--signal-color-neutral-3);
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: all 0.2s;
  flex-shrink: 0;
}

.signal-radio-circle.checked {
  border-color: var(--signal-color-info-5);
}

.signal-radio-dot {
  width: 10px;
  height: 10px;
  border-radius: 50%;
  background: var(--signal-color-info-5);
}

.signal-radio-label {
  color: var(--signal-color-secondary-7);
  user-select: none;
}
```

### Switch

#### Switch.tsx

```tsx
'use client';

import { useState } from 'react';
import clsx from 'clsx';
import './Switch.css';

export interface SwitchProps {
  label?: string;
  labelPosition?: 'left' | 'right';
  disabled?: boolean;
  checked?: boolean;
  onChange?: (checked: boolean) => void;
  className?: string;
}

export function Switch({
  label,
  labelPosition = 'right',
  disabled = false,
  checked = false,
  onChange,
  className,
}: SwitchProps) {
  const [isOn, setIsOn] = useState(checked);

  const toggleSwitch = () => {
    if (disabled) return;
    const newValue = !isOn;
    setIsOn(newValue);
    onChange?.(newValue);
  };

  return (
    <div className={clsx('signal-switch-root', className)}>
      {label && labelPosition === 'left' && (
        <span className="signal-switch-label">{label}</span>
      )}
      <button
        type="button"
        role="switch"
        aria-checked={isOn}
        disabled={disabled}
        className={clsx('signal-switch-track', { on: isOn, disabled })}
        onClick={toggleSwitch}
      >
        <span className={clsx('signal-switch-thumb', { on: isOn })} />
      </button>
      {label && labelPosition === 'right' && (
        <span className="signal-switch-label">{label}</span>
      )}
    </div>
  );
}
```

#### Switch.css

```css
.signal-switch-root {
  display: flex;
  align-items: center;
  gap: 8px;
}

.signal-switch-track {
  width: 44px;
  height: 24px;
  border-radius: 12px;
  background: var(--signal-color-neutral-3);
  border: none;
  cursor: pointer;
  position: relative;
  transition: background 0.2s;
  padding: 0;
  flex-shrink: 0;
}

.signal-switch-track.on {
  background: var(--signal-color-valid-5);
}

.signal-switch-track.disabled {
  opacity: 0.5;
  cursor: not-allowed;
}

.signal-switch-thumb {
  position: absolute;
  top: 2px;
  left: 2px;
  width: 20px;
  height: 20px;
  border-radius: 50%;
  background: var(--signal-color-neutral-0);
  box-shadow: 0 1px 3px rgba(0, 0, 0, 0.2);
  transition: transform 0.2s;
}

.signal-switch-thumb.on {
  transform: translateX(20px);
}

.signal-switch-label {
  font-family: var(--font-family-body);
  font-size: var(--font-size-body-1);
  color: var(--signal-color-secondary-7);
  user-select: none;
}
```

### Tab

#### Tab.tsx

```tsx
'use client';

import { useState } from 'react';
import clsx from 'clsx';
import './Tab.css';

export interface TabItem {
  label: string;
}

export interface TabProps {
  tabs: TabItem[];
  onChange?: (index: number) => void;
  defaultIndex?: number;
  className?: string;
}

export function Tab({ tabs, onChange, defaultIndex = 0, className }: TabProps) {
  const [activeTab, setActiveTab] = useState(defaultIndex);

  const handleClick = (index: number) => {
    setActiveTab(index);
    onChange?.(index);
  };

  return (
    <div className={clsx('signal-tab-root', className)}>
      <div className="signal-tab-nav">
        {tabs.map((tab, index) => (
          <button
            key={index}
            type="button"
            className={clsx('signal-tab-item', { 'signal-tab-active': activeTab === index })}
            onClick={() => handleClick(index)}
          >
            <span>{tab.label}</span>
          </button>
        ))}
      </div>
    </div>
  );
}
```

#### Tab.css

```css
.signal-tab-root {
  width: 100%;
}

.signal-tab-nav {
  display: flex;
  border-bottom: 2px solid var(--signal-color-neutral-2);
}

.signal-tab-item {
  flex: 1;
  padding: 12px 16px;
  font-family: var(--font-family-body);
  font-size: var(--font-size-body-1);
  font-weight: 600;
  color: var(--signal-color-neutral-5);
  background: none;
  border: none;
  border-bottom: 2px solid transparent;
  margin-bottom: -2px;
  cursor: pointer;
  transition: all 0.2s;
  text-align: center;
}

.signal-tab-item:hover {
  color: var(--signal-color-primary-5);
}

.signal-tab-item.signal-tab-active {
  color: var(--signal-color-primary-5);
  border-bottom-color: var(--signal-color-primary-5);
}
```

### TextArea

#### TextArea.tsx

```tsx
'use client';

import { ChangeEvent, useRef, useState } from 'react';
import clsx from 'clsx';
import './TextArea.css';

export interface TextAreaProps {
  name?: string;
  placeholder?: string;
  variant?: 'primary' | 'secondary';
  labelColor?: string;
  supportText?: string;
  successText?: string;
  disabled?: boolean;
  required?: boolean;
  minRows?: number;
  maxChar?: number;
  value?: string;
  onChange?: (value: string) => void;
  className?: string;
}

export function TextArea({
  name,
  placeholder,
  variant = 'primary',
  labelColor,
  supportText,
  successText,
  disabled,
  required,
  minRows = 4,
  maxChar = 255,
  value,
  onChange,
  className,
}: TextAreaProps) {
  const [charCount, setCharCount] = useState(value?.length || 0);
  const textareaRef = useRef<HTMLTextAreaElement>(null);

  const handleChange = (e: ChangeEvent<HTMLTextAreaElement>) => {
    const val = e.target.value;
    setCharCount(val.length);
    onChange?.(val);
  };

  const handleClear = () => {
    if (textareaRef.current) {
      textareaRef.current.value = '';
      setCharCount(0);
      onChange?.('');
    }
  };

  const textareaClass = clsx('signal-textarea-input', `textarea-${variant}`, {
    'textarea-error': !!supportText,
    'textarea-success': !!successText,
    'textarea-disabled': disabled,
  });

  return (
    <div className={clsx('signal-textarea-root', className)}>
      {name && (
        <label className="signal-textarea-label" style={{ color: labelColor }}>
          {name}
          {required && <span className="signal-textarea-required">*</span>}
        </label>
      )}
      <div className="signal-textarea-wrapper">
        <textarea
          ref={textareaRef}
          className={textareaClass}
          placeholder={placeholder}
          disabled={disabled}
          maxLength={maxChar}
          rows={minRows}
          defaultValue={value}
          onChange={handleChange}
          aria-invalid={!!supportText}
        />
        {charCount > 0 && !disabled && (
          <button type="button" className="signal-textarea-clear" onClick={handleClear} aria-label="Clear text">
            ✕
          </button>
        )}
      </div>
      <div className="signal-textarea-footer">
        <div>
          {supportText && <p className="signal-textarea-error">{supportText}</p>}
          {successText && !supportText && <p className="signal-textarea-success">{successText}</p>}
        </div>
        <p className="signal-textarea-count">{charCount}/{maxChar}</p>
      </div>
    </div>
  );
}
```

#### TextArea.css

```css
.signal-textarea-root {
  display: flex;
  flex-direction: column;
  gap: 4px;
  width: 100%;
}

.signal-textarea-label {
  font-family: var(--font-family-body);
  font-size: var(--font-size-body-2);
  font-weight: 600;
  color: var(--signal-color-secondary-7);
}

.signal-textarea-required {
  color: var(--signal-color-error-5);
  margin-left: 2px;
}

.signal-textarea-wrapper {
  position: relative;
}

.signal-textarea-input {
  font-family: var(--font-family-body);
  font-size: var(--font-size-body-1);
  padding: 10px 14px;
  border: 1px solid var(--signal-color-neutral-3);
  border-radius: var(--radius-md);
  outline: none;
  transition: border-color 0.2s;
  background: var(--signal-color-neutral-0);
  color: var(--signal-color-secondary-7);
  width: 100%;
  resize: none;
}

.signal-textarea-input::placeholder {
  color: var(--signal-color-secondary-4);
}

.signal-textarea-input.textarea-primary:focus {
  border-color: var(--signal-color-primary-5);
}

.signal-textarea-input.textarea-secondary:focus {
  border-color: var(--signal-color-secondary-7);
}

.signal-textarea-input.textarea-error {
  border-color: var(--signal-color-error-5);
}

.signal-textarea-input.textarea-success {
  border-color: var(--signal-color-valid-5);
}

.signal-textarea-input.textarea-disabled {
  background: var(--signal-color-neutral-1);
  color: var(--signal-color-secondary-4);
  cursor: not-allowed;
}

.signal-textarea-clear {
  position: absolute;
  top: 8px;
  right: 8px;
  background: var(--signal-color-neutral-2);
  border: none;
  border-radius: 50%;
  width: 20px;
  height: 20px;
  font-size: 10px;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  color: var(--signal-color-neutral-5);
}

.signal-textarea-footer {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.signal-textarea-error {
  font-size: var(--font-size-body-2);
  color: var(--signal-color-error-5);
  margin: 0;
}

.signal-textarea-success {
  font-size: var(--font-size-body-2);
  color: var(--signal-color-valid-5);
  margin: 0;
}

.signal-textarea-count {
  font-size: var(--font-size-label);
  color: var(--signal-color-neutral-4);
  margin: 0;
}
```

### OTP

#### OTP.tsx

```tsx
'use client';

import { useEffect, useRef, useState } from 'react';
import clsx from 'clsx';
import './OTP.css';

export interface OTPProps {
  length?: 4 | 5 | 6 | 7 | 8;
  value?: string;
  onChange: (otp: string) => void;
  isPassword?: boolean;
  disabled?: boolean;
  placeholder?: string;
  className?: string;
}

export function OTP({
  length = 6,
  value = '',
  onChange,
  isPassword = false,
  disabled = false,
  placeholder = '',
  className,
}: OTPProps) {
  const [otpValues, setOtpValues] = useState<string[]>(
    Array.from({ length }, (_, i) => value[i] || ''),
  );
  const inputRefs = useRef<(HTMLInputElement | null)[]>([]);

  useEffect(() => {
    if (value.length === length) {
      setOtpValues(Array.from(value));
    }
  }, [value, length]);

  const handleChange = (val: string, index: number) => {
    if (!/^[0-9]?$/.test(val)) return;

    const newValues = [...otpValues];
    newValues[index] = val;
    setOtpValues(newValues);
    onChange(newValues.join(''));

    if (val && index < length - 1) {
      inputRefs.current[index + 1]?.focus();
    }
  };

  const handleKeyDown = (e: React.KeyboardEvent, index: number) => {
    if (e.key === 'Backspace' && !otpValues[index] && index > 0) {
      inputRefs.current[index - 1]?.focus();
    }
  };

  return (
    <div className={clsx('signal-otp-root', className)}>
      {otpValues.map((val, index) => (
        <input
          key={index}
          ref={(el) => { inputRefs.current[index] = el; }}
          type={isPassword ? 'password' : 'tel'}
          inputMode="numeric"
          maxLength={1}
          value={val}
          placeholder={placeholder}
          disabled={disabled}
          className={clsx('signal-otp-input', { disabled })}
          onChange={(e) => handleChange(e.target.value, index)}
          onKeyDown={(e) => handleKeyDown(e, index)}
          aria-label={`OTP digit ${index + 1}`}
        />
      ))}
    </div>
  );
}
```

#### OTP.css

```css
.signal-otp-root {
  display: flex;
  gap: 8px;
}

.signal-otp-input {
  width: 44px;
  height: 48px;
  text-align: center;
  font-family: var(--font-family-body);
  font-size: var(--font-size-heading-5);
  font-weight: 600;
  border: 1px solid var(--signal-color-neutral-3);
  border-radius: var(--radius-md);
  outline: none;
  transition: border-color 0.2s;
  background: var(--signal-color-neutral-0);
  color: var(--signal-color-secondary-7);
}

.signal-otp-input:focus {
  border-color: var(--signal-color-primary-5);
  box-shadow: 0 0 0 2px var(--signal-color-primary-1);
}

.signal-otp-input.disabled {
  background: var(--signal-color-neutral-1);
  color: var(--signal-color-secondary-4);
  cursor: not-allowed;
}

.signal-otp-input::placeholder {
  color: var(--signal-color-neutral-3);
}
```

### Breadcrumb

#### Breadcrumb.tsx

```tsx
'use client';

import clsx from 'clsx';
import './Breadcrumb.css';

export interface BreadcrumbItem {
  name: string;
  href: string;
}

export interface BreadcrumbProps {
  items: BreadcrumbItem[];
  className?: string;
}

export function Breadcrumb({ items, className }: BreadcrumbProps) {
  return (
    <nav className={clsx('signal-breadcrumb-root', className)} aria-label="Breadcrumb">
      <ol className="signal-breadcrumb-list">
        {items.map((item, index) => {
          const isLast = index === items.length - 1;
          return (
            <li key={index} className="signal-breadcrumb-item">
              {isLast ? (
                <span className="signal-breadcrumb-current" aria-current="page">
                  {item.name}
                </span>
              ) : (
                <>
                  <a href={item.href} className="signal-breadcrumb-link">
                    {item.name}
                  </a>
                  <span className="signal-breadcrumb-sep" aria-hidden="true">/</span>
                </>
              )}
            </li>
          );
        })}
      </ol>
    </nav>
  );
}
```

#### Breadcrumb.css

```css
.signal-breadcrumb-root {
  font-family: var(--font-family-body);
  font-size: var(--font-size-body-2);
}

.signal-breadcrumb-list {
  display: flex;
  align-items: center;
  gap: 8px;
  list-style: none;
  padding: 0;
  margin: 0;
}

.signal-breadcrumb-item {
  display: flex;
  align-items: center;
  gap: 8px;
}

.signal-breadcrumb-link {
  color: var(--signal-color-info-5);
  text-decoration: none;
}

.signal-breadcrumb-link:hover {
  text-decoration: underline;
}

.signal-breadcrumb-sep {
  color: var(--signal-color-neutral-4);
}

.signal-breadcrumb-current {
  color: var(--signal-color-neutral-5);
}
```

### BottomSheet

#### BottomSheet.tsx

```tsx
'use client';

import { ReactNode, useCallback, useEffect, useRef, useState } from 'react';
import clsx from 'clsx';
import './BottomSheet.css';

export interface BottomSheetProps {
  isVisible: boolean;
  onClose: () => void;
  maxHeight?: string;
  dismissable?: boolean;
  title?: string;
  children: ReactNode;
  className?: string;
}

export function BottomSheet({
  isVisible,
  onClose,
  maxHeight = '90%',
  dismissable = true,
  title,
  children,
  className,
}: BottomSheetProps) {
  const [isAnimating, setIsAnimating] = useState(false);
  const [isRendered, setIsRendered] = useState(false);
  const timeoutRef = useRef<ReturnType<typeof setTimeout> | null>(null);

  const handleClose = useCallback(() => {
    setIsAnimating(false);
    if (timeoutRef.current) clearTimeout(timeoutRef.current);
    timeoutRef.current = setTimeout(() => {
      setIsRendered(false);
      onClose();
    }, 300);
  }, [onClose]);

  const handleOverlayClick = (e: React.MouseEvent<HTMLDivElement>) => {
    if (dismissable && e.target === e.currentTarget) {
      handleClose();
    }
  };

  useEffect(() => {
    const handleKeyDown = (e: KeyboardEvent) => {
      if (e.key === 'Escape') handleClose();
    };

    if (isVisible) {
      setIsRendered(true);
      document.body.style.overflow = 'hidden';
      document.addEventListener('keydown', handleKeyDown);
      requestAnimationFrame(() => setIsAnimating(true));
    } else {
      handleClose();
    }

    return () => {
      document.body.style.overflow = '';
      document.removeEventListener('keydown', handleKeyDown);
      if (timeoutRef.current) clearTimeout(timeoutRef.current);
    };
  }, [isVisible, handleClose]);

  if (!isRendered && !isVisible) return null;

  return (
    <div
      className={clsx('signal-bottomsheet-overlay', { visible: isAnimating })}
      onClick={handleOverlayClick}
      role="dialog"
      aria-modal="true"
    >
      <div
        className={clsx('signal-bottomsheet-container', { visible: isAnimating }, className)}
        onClick={(e) => e.stopPropagation()}
        style={{ maxHeight }}
      >
        <div className="signal-bottomsheet-header">
          {title && <h3 className="signal-bottomsheet-title">{title}</h3>}
          <button className="signal-bottomsheet-close" onClick={handleClose} aria-label="Close">
            ✕
          </button>
        </div>
        <div className="signal-bottomsheet-content">
          {children}
        </div>
      </div>
    </div>
  );
}
```

#### BottomSheet.css

```css
.signal-bottomsheet-overlay {
  position: fixed;
  inset: 0;
  background: rgba(0, 0, 0, 0);
  z-index: 1000;
  display: flex;
  align-items: flex-end;
  justify-content: center;
  transition: background 0.3s;
}

.signal-bottomsheet-overlay.visible {
  background: rgba(0, 0, 0, 0.5);
}

.signal-bottomsheet-container {
  width: 100%;
  max-width: 480px;
  background: var(--signal-color-neutral-0);
  border-radius: var(--radius-xl) var(--radius-xl) 0 0;
  transform: translateY(100%);
  transition: transform 0.3s ease;
  overflow-y: auto;
}

.signal-bottomsheet-container.visible {
  transform: translateY(0);
}

.signal-bottomsheet-header {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 16px 20px;
  border-bottom: 1px solid var(--signal-color-neutral-2);
}

.signal-bottomsheet-title {
  font-family: var(--font-family-heading);
  font-size: var(--font-size-heading-6);
  font-weight: 700;
  color: var(--signal-color-secondary-7);
  margin: 0;
}

.signal-bottomsheet-close {
  background: none;
  border: none;
  font-size: 18px;
  cursor: pointer;
  color: var(--signal-color-neutral-5);
  width: 28px;
  height: 28px;
  display: flex;
  align-items: center;
  justify-content: center;
  border-radius: var(--radius-full);
}

.signal-bottomsheet-close:hover {
  background: var(--signal-color-neutral-1);
}

.signal-bottomsheet-content {
  padding: 20px;
}
```
