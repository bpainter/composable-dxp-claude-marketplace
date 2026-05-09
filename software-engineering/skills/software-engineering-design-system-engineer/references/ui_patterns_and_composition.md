---
type: reference
project: skills-library
scope: skill
plugin: software-engineering
parent: "[[design-system-engineer]]"
tags: [type/reference, plugin/software-engineering, scope/skill, topic/software]
status: active
---
# UI Patterns & Composition

## shadcn/ui Architecture & Customization

### How shadcn/ui Works

shadcn/ui is not a component library you install via npm. It's a collection of copy-paste-ready React components built on:

1. **Radix UI**: Unstyled, headless components providing accessibility and interactivity (Modal, Popover, Combobox, etc.)
2. **Tailwind CSS**: Styling layer applied via class composition
3. **class-variance-authority (CVA)**: Type-safe variant system for managing component props

The copy-paste model means:
- You own the component code in your project (`components/ui/button.tsx`)
- You can customize freely without waiting for upstream updates
- No version conflicts or breaking dependency changes
- Full control over component behavior and styling

### Project Structure

```
src/
├── components/
│   ├── ui/                      # shadcn/ui base components (copy-pasted)
│   │   ├── button.tsx
│   │   ├── input.tsx
│   │   ├── dialog.tsx
│   │   ├── table.tsx
│   │   └── ... other shadcn components
│   ├── form/                    # Custom form components (wrappers around shadcn)
│   │   ├── form-field.tsx
│   │   ├── form-input.tsx
│   │   ├── form-select.tsx
│   │   └── form-error.tsx
│   ├── patterns/                # Reusable UI patterns (compositions)
│   │   ├── data-table.tsx
│   │   ├── dashboard-sidebar.tsx
│   │   ├── settings-form.tsx
│   │   └── modal-form.tsx
│   └── layouts/                 # Page layouts
│       ├── dashboard-layout.tsx
│       └── auth-layout.tsx
└── stories/                     # Storybook stories
```

### Customization Pattern 1: Variant Extension with CVA

Use `class-variance-authority` to safely extend component variants:

```typescript
// components/ui/button-extended.tsx
import * as React from 'react'
import { Slot } from '@radix-ui/react-slot'
import { cva, type VariantProps } from 'class-variance-authority'

import { cn } from '@/lib/utils'

const buttonVariants = cva(
  'inline-flex items-center justify-center whitespace-nowrap rounded-md text-sm font-medium ring-offset-background transition-colors focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-ring focus-visible:ring-offset-2 disabled:pointer-events-none disabled:opacity-50',
  {
    variants: {
      // Visual appearance
      appearance: {
        primary:
          'bg-primary text-text-on-primary hover:bg-primary/90 active:bg-primary/80',
        secondary:
          'bg-surface-muted text-text border border-surface-border hover:bg-surface-muted/80',
        ghost: 'hover:bg-surface-muted active:bg-surface-muted/80',
        ghost-destructive: 'hover:bg-red-50 text-destructive active:text-red-700',
        destructive:
          'bg-destructive text-white hover:bg-destructive/90 active:bg-destructive/80',
        outline: 'border border-surface-border bg-surface hover:bg-surface-muted',
      },
      // Size
      size: {
        xs: 'h-8 px-3 text-xs',
        sm: 'h-9 px-3 text-sm',
        md: 'h-10 px-4 text-base',
        lg: 'h-11 px-8 text-base',
        xl: 'h-12 px-8 text-lg',
        icon: 'h-10 w-10',
        'icon-sm': 'h-8 w-8',
      },
      // Width
      width: {
        auto: 'w-auto',
        full: 'w-full',
        fit: 'w-fit',
      },
    },
    defaultVariants: {
      appearance: 'primary',
      size: 'md',
      width: 'auto',
    },
  }
)

export interface ButtonProps
  extends React.ButtonHTMLAttributes<HTMLButtonElement>,
    VariantProps<typeof buttonVariants> {
  asChild?: boolean
}

const Button = React.forwardRef<HTMLButtonElement, ButtonProps>(
  (
    { appearance, size, width, asChild = false, className, ...props },
    ref
  ) => {
    const Comp = asChild ? Slot : 'button'
    return (
      <Comp
        className={cn(buttonVariants({ appearance, size, width, className }))}
        ref={ref}
        {...props}
      />
    )
  }
)
Button.displayName = 'Button'

export { Button, buttonVariants }
```

Benefits:
- Type-safe variant props with autocomplete
- Compile-time validation (TypeScript catches invalid combinations)
- Easy to extend or modify
- No runtime overhead

### Customization Pattern 2: Compound Components

Combine multiple shadcn components into a cohesive API:

```typescript
// components/form/form-field.tsx
import React from 'react'
import { Input } from '@/components/ui/input'
import { Label } from '@/components/ui/label'
import { cn } from '@/lib/utils'

interface FormFieldProps {
  label?: React.ReactNode
  error?: string
  hint?: string
  required?: boolean
  children: React.ReactNode
  className?: string
}

export const FormField = React.forwardRef<
  HTMLDivElement,
  FormFieldProps
>(({ label, error, hint, required, children, className }, ref) => {
  const id = React.useId()

  return (
    <div ref={ref} className={cn('flex flex-col gap-2', className)}>
      {label && (
        <Label htmlFor={id}>
          {label}
          {required && <span className="text-destructive ml-1">*</span>}
        </Label>
      )}
      {React.isValidElement(children)
        ? React.cloneElement(children as React.ReactElement<any>, { id })
        : children}
      {hint && !error && (
        <p className="text-xs text-text-secondary">{hint}</p>
      )}
      {error && <p className="text-xs text-destructive">{error}</p>}
    </div>
  )
})
FormField.displayName = 'FormField'
```

Usage:
```typescript
<FormField
  label="Email"
  hint="We'll never share this"
  error={errors.email}
  required
>
  <Input type="email" placeholder="you@example.com" />
</FormField>
```

### Customization Pattern 3: Wrapper for Logic

Enhance shadcn components with business logic:

```typescript
// components/form/form-input.tsx
import React from 'react'
import { Input } from '@/components/ui/input'
import { cn } from '@/lib/utils'

interface FormInputProps
  extends React.InputHTMLAttributes<HTMLInputElement> {
  // Custom props
  characterLimit?: number
  showCharacterCount?: boolean
  icon?: React.ReactNode
  loading?: boolean
}

export const FormInput = React.forwardRef<
  HTMLInputElement,
  FormInputProps
>(
  (
    {
      characterLimit,
      showCharacterCount,
      icon,
      loading,
      className,
      onChange,
      value,
      ...props
    },
    ref
  ) => {
    const [charCount, setCharCount] = React.useState(0)

    const handleChange = (e: React.ChangeEvent<HTMLInputElement>) => {
      setCharCount(e.target.value.length)
      onChange?.(e)
    }

    return (
      <div className="relative">
        {icon && (
          <div className="absolute left-3 top-1/2 -translate-y-1/2 text-text-secondary">
            {icon}
          </div>
        )}
        <Input
          ref={ref}
          value={value}
          onChange={handleChange}
          maxLength={characterLimit}
          className={cn(
            icon && 'pl-10',
            loading && 'opacity-50',
            className
          )}
          {...props}
        />
        {showCharacterCount && characterLimit && (
          <p className="text-xs text-text-secondary mt-1">
            {charCount} / {characterLimit}
          </p>
        )}
      </div>
    )
  }
)
FormInput.displayName = 'FormInput'
```

### Decision Tree: Wrap vs. Build Custom

```
Does a Radix primitive exist for this?
├─ No
│  └─ Build custom using headless library (cmdk, react-hook-form, etc.)
│
└─ Yes (e.g., Dialog, Popover, Combobox)
   ├─ Does shadcn have a component for it?
   │  ├─ No
   │  │  └─ Wrap Radix primitive with Tailwind styling
   │  │
   │  └─ Yes
   │     ├─ Just need a new variant?
   │     │  └─ Extend with CVA (don't wrap)
   │     │
   │     └─ Need to combine with other components?
   │        └─ Create compound component wrapping shadcn
```

---

## Common UI Patterns

### Pattern 1: Data Table

The most complex pattern. Handle sorting, filtering, pagination, row selection.

**Components Used:**
- shadcn `Table` (basic rendering)
- shadcn `Button` (pagination, sort controls)
- shadcn `Checkbox` (row selection)
- shadcn `Input` (search/filter)
- Optional: TanStack Table v8 for large datasets

**Basic Implementation:**

```typescript
// components/patterns/data-table.tsx
import React from 'react'
import {
  Table,
  TableBody,
  TableCell,
  TableHead,
  TableHeader,
  TableRow,
} from '@/components/ui/table'
import { Button } from '@/components/ui/button'
import { Input } from '@/components/ui/input'
import { Checkbox } from '@/components/ui/checkbox'
import {
  ChevronUp,
  ChevronDown,
  ChevronsUpDown,
  ChevronLeft,
  ChevronRight,
} from 'lucide-react'
import { cn } from '@/lib/utils'

export interface DataTableColumn<T> {
  key: keyof T
  label: string
  sortable?: boolean
  filterable?: boolean
  render?: (value: any, row: T) => React.ReactNode
  width?: string
}

interface DataTableProps<T extends { id: string | number }> {
  data: T[]
  columns: DataTableColumn<T>[]
  onRowClick?: (row: T) => void
  onRowSelect?: (selected: (string | number)[]) => void
  selectable?: boolean
  pageable?: boolean
  itemsPerPage?: number
}

export function DataTable<T extends { id: string | number }>({
  data,
  columns,
  onRowClick,
  onRowSelect,
  selectable = true,
  pageable = true,
  itemsPerPage = 10,
}: DataTableProps<T>) {
  // State management
  const [sortKey, setSortKey] = React.useState<keyof T | null>(null)
  const [sortDir, setSortDir] = React.useState<'asc' | 'desc'>('asc')
  const [selectedRows, setSelectedRows] = React.useState<Set<string | number>>(
    new Set()
  )
  const [currentPage, setCurrentPage] = React.useState(1)
  const [filters, setFilters] = React.useState<Partial<Record<keyof T, string>>>({})

  // Filtering logic
  const filtered = React.useMemo(() => {
    return data.filter((row) => {
      return Object.entries(filters).every(([key, value]) => {
        if (!value) return true
        const rowValue = String(row[key as keyof T]).toLowerCase()
        return rowValue.includes(value.toLowerCase())
      })
    })
  }, [data, filters])

  // Sorting logic
  const sorted = React.useMemo(() => {
    if (!sortKey) return filtered
    return [...filtered].sort((a, b) => {
      const aVal = a[sortKey]
      const bVal = b[sortKey]
      const cmp =
        aVal < bVal ? -1 : aVal > bVal ? 1 : 0
      return sortDir === 'asc' ? cmp : -cmp
    })
  }, [filtered, sortKey, sortDir])

  // Pagination logic
  const totalPages = Math.ceil(sorted.length / itemsPerPage)
  const paginatedData = pageable
    ? sorted.slice(
        (currentPage - 1) * itemsPerPage,
        currentPage * itemsPerPage
      )
    : sorted

  // Selection helpers
  const isAllSelected =
    selectable && paginatedData.length > 0 &&
    paginatedData.every((row) => selectedRows.has(row.id))
  const isSomeSelected =
    selectable &&
    paginatedData.some((row) => selectedRows.has(row.id)) &&
    !isAllSelected

  const toggleSelectAll = () => {
    const newSelected = new Set(selectedRows)
    paginatedData.forEach((row) => {
      if (isAllSelected) {
        newSelected.delete(row.id)
      } else {
        newSelected.add(row.id)
      }
    })
    setSelectedRows(newSelected)
    onRowSelect?.(Array.from(newSelected))
  }

  const toggleSelectRow = (id: string | number) => {
    const newSelected = new Set(selectedRows)
    if (newSelected.has(id)) {
      newSelected.delete(id)
    } else {
      newSelected.add(id)
    }
    setSelectedRows(newSelected)
    onRowSelect?.(Array.from(newSelected))
  }

  // Sort handler
  const handleSort = (key: keyof T) => {
    const col = columns.find((c) => c.key === key)
    if (!col?.sortable) return

    if (sortKey === key) {
      setSortDir(sortDir === 'asc' ? 'desc' : 'asc')
    } else {
      setSortKey(key)
      setSortDir('asc')
    }
  }

  return (
    <div className="space-y-4">
      {/* Filters */}
      <div className="flex gap-2">
        {columns
          .filter((c) => c.filterable)
          .map((col) => (
            <Input
              key={String(col.key)}
              placeholder={`Filter by ${col.label}...`}
              value={filters[col.key] || ''}
              onChange={(e) =>
                setFilters({
                  ...filters,
                  [col.key]: e.target.value,
                })
              }
              className="max-w-xs"
            />
          ))}
      </div>

      {/* Table */}
      <Table>
        <TableHeader>
          <TableRow>
            {selectable && (
              <TableHead className="w-12">
                <Checkbox
                  checked={isAllSelected}
                  indeterminate={isSomeSelected}
                  onChange={toggleSelectAll}
                />
              </TableHead>
            )}
            {columns.map((col) => (
              <TableHead
                key={String(col.key)}
                style={{ width: col.width }}
                className={col.sortable ? 'cursor-pointer' : ''}
                onClick={() => col.sortable && handleSort(col.key)}
              >
                <div className="flex items-center gap-2">
                  {col.label}
                  {col.sortable && (
                    <>
                      {sortKey === col.key &&
                        (sortDir === 'asc' ? (
                          <ChevronUp className="w-4 h-4" />
                        ) : (
                          <ChevronDown className="w-4 h-4" />
                        ))}
                      {sortKey !== col.key && (
                        <ChevronsUpDown className="w-4 h-4 opacity-30" />
                      )}
                    </>
                  )}
                </div>
              </TableHead>
            ))}
          </TableRow>
        </TableHeader>
        <TableBody>
          {paginatedData.length === 0 ? (
            <TableRow>
              <TableCell
                colSpan={columns.length + (selectable ? 1 : 0)}
                className="text-center py-8 text-text-secondary"
              >
                No data found
              </TableCell>
            </TableRow>
          ) : (
            paginatedData.map((row) => (
              <TableRow
                key={row.id}
                onClick={() => onRowClick?.(row)}
                className={cn(
                  onRowClick && 'cursor-pointer hover:bg-surface-muted',
                  selectedRows.has(row.id) && 'bg-surface-muted'
                )}
              >
                {selectable && (
                  <TableCell onClick={(e) => e.stopPropagation()}>
                    <Checkbox
                      checked={selectedRows.has(row.id)}
                      onChange={() => toggleSelectRow(row.id)}
                    />
                  </TableCell>
                )}
                {columns.map((col) => (
                  <TableCell key={String(col.key)}>
                    {col.render
                      ? col.render(row[col.key], row)
                      : String(row[col.key])}
                  </TableCell>
                ))}
              </TableRow>
            ))
          )}
        </TableBody>
      </Table>

      {/* Pagination */}
      {pageable && totalPages > 1 && (
        <div className="flex items-center justify-between">
          <p className="text-sm text-text-secondary">
            Showing {(currentPage - 1) * itemsPerPage + 1} to{' '}
            {Math.min(currentPage * itemsPerPage, sorted.length)} of{' '}
            {sorted.length} results
          </p>
          <div className="flex gap-2">
            <Button
              appearance="outline"
              size="sm"
              onClick={() => setCurrentPage((p) => Math.max(1, p - 1))}
              disabled={currentPage === 1}
            >
              <ChevronLeft className="w-4 h-4" />
            </Button>
            <Button appearance="outline" size="sm" disabled>
              {currentPage} / {totalPages}
            </Button>
            <Button
              appearance="outline"
              size="sm"
              onClick={() => setCurrentPage((p) => Math.min(totalPages, p + 1))}
              disabled={currentPage === totalPages}
            >
              <ChevronRight className="w-4 h-4" />
            </Button>
          </div>
        </div>
      )}
    </div>
  )
}
```

### Pattern 2: Dashboard Layout

Three-part layout: fixed sidebar, top header, main content area.

```typescript
// components/layouts/dashboard-layout.tsx
import React from 'react'
import { Sidebar } from './sidebar'
import { Header } from './header'
import { cn } from '@/lib/utils'

interface DashboardLayoutProps {
  children: React.ReactNode
  sidebarCollapsed?: boolean
  onSidebarToggle?: (collapsed: boolean) => void
}

export function DashboardLayout({
  children,
  sidebarCollapsed = false,
  onSidebarToggle,
}: DashboardLayoutProps) {
  const [collapsed, setCollapsed] = React.useState(sidebarCollapsed)

  const handleToggle = (newState: boolean) => {
    setCollapsed(newState)
    onSidebarToggle?.(newState)
  }

  return (
    <div className="flex h-screen overflow-hidden">
      {/* Sidebar */}
      <Sidebar
        collapsed={collapsed}
        onToggle={handleToggle}
      />

      {/* Main content area */}
      <div className="flex-1 flex flex-col overflow-hidden">
        {/* Header */}
        <Header onMenuClick={() => handleToggle(!collapsed)} />

        {/* Content */}
        <main className={cn(
          'flex-1 overflow-auto bg-surface',
          'p-gutter'
        )}>
          <div className="max-w-7xl mx-auto">
            {children}
          </div>
        </main>
      </div>
    </div>
  )
}
```

### Pattern 3: Multi-Step Form (Wizard)

Linear, step-by-step form progression.

```typescript
// components/patterns/form-wizard.tsx
import React from 'react'
import { Button } from '@/components/ui/button'
import { Progress } from '@/components/ui/progress'

interface WizardStep {
  id: string
  title: string
  description?: string
  content: React.ReactNode
  validate?: () => boolean | Promise<boolean>
}

interface FormWizardProps {
  steps: WizardStep[]
  onComplete: () => void | Promise<void>
  onCancel: () => void
}

export function FormWizard({
  steps,
  onComplete,
  onCancel,
}: FormWizardProps) {
  const [currentStep, setCurrentStep] = React.useState(0)
  const [loading, setLoading] = React.useState(false)

  const step = steps[currentStep]
  const isLastStep = currentStep === steps.length - 1
  const progress = ((currentStep + 1) / steps.length) * 100

  const handleNext = async () => {
    setLoading(true)
    try {
      const isValid = await step.validate?.()
      if (isValid === false) {
        setLoading(false)
        return
      }
      if (isLastStep) {
        await onComplete()
      } else {
        setCurrentStep((prev) => prev + 1)
      }
    } finally {
      setLoading(false)
    }
  }

  const handlePrevious = () => {
    setCurrentStep((prev) => Math.max(0, prev - 1))
  }

  return (
    <div className="w-full max-w-2xl mx-auto space-y-8">
      {/* Progress bar */}
      <div className="space-y-2">
        <div className="flex justify-between items-center">
          <h2 className="text-heading-lg">{step.title}</h2>
          <span className="text-sm text-text-secondary">
            Step {currentStep + 1} of {steps.length}
          </span>
        </div>
        <Progress value={progress} />
        {step.description && (
          <p className="text-text-secondary">{step.description}</p>
        )}
      </div>

      {/* Step content */}
      <div className="bg-surface border border-surface-border rounded-lg p-card-gap">
        {step.content}
      </div>

      {/* Navigation */}
      <div className="flex justify-between pt-4">
        <Button
          appearance="outline"
          onClick={onCancel}
        >
          Cancel
        </Button>
        <div className="flex gap-3">
          <Button
            appearance="secondary"
            onClick={handlePrevious}
            disabled={currentStep === 0}
          >
            Previous
          </Button>
          <Button
            appearance="primary"
            onClick={handleNext}
            loading={loading}
          >
            {isLastStep ? 'Complete' : 'Next'}
          </Button>
        </div>
      </div>
    </div>
  )
}
```

### Pattern 4: Settings Page

Tabbed navigation with form sections.

```typescript
// components/patterns/settings-page.tsx
import React from 'react'
import { Tabs, TabsContent, TabsList, TabsTrigger } from '@/components/ui/tabs'
import { Button } from '@/components/ui/button'
import { Separator } from '@/components/ui/separator'

interface SettingSection {
  id: string
  label: string
  icon?: React.ReactNode
  content: React.ReactNode
  onSave?: () => Promise<void>
}

interface SettingsPageProps {
  sections: SettingSection[]
  title?: string
}

export function SettingsPage({
  sections,
  title = 'Settings',
}: SettingsPageProps) {
  const [activeSectionId, setActiveSectionId] = React.useState(
    sections[0]?.id
  )
  const [loading, setLoading] = React.useState(false)

  const activeSection = sections.find((s) => s.id === activeSectionId)

  const handleSave = async () => {
    if (!activeSection?.onSave) return
    setLoading(true)
    try {
      await activeSection.onSave()
    } finally {
      setLoading(false)
    }
  }

  return (
    <div className="space-y-8">
      <div>
        <h1 className="text-heading-2xl">{title}</h1>
      </div>

      <Tabs value={activeSectionId} onValueChange={setActiveSectionId}>
        <TabsList>
          {sections.map((section) => (
            <TabsTrigger key={section.id} value={section.id}>
              {section.icon}
              {section.label}
            </TabsTrigger>
          ))}
        </TabsList>

        {sections.map((section) => (
          <TabsContent key={section.id} value={section.id}>
            <div className="space-y-6">
              <Separator />
              <div className="bg-surface border border-surface-border rounded-lg p-card-gap">
                {section.content}
              </div>
              {section.onSave && (
                <div className="flex justify-end gap-3">
                  <Button appearance="outline">Cancel</Button>
                  <Button
                    appearance="primary"
                    onClick={handleSave}
                    disabled={!activeSection?.onSave}
                    loading={loading}
                  >
                    Save Changes
                  </Button>
                </div>
              )}
            </div>
          </TabsContent>
        ))}
      </Tabs>
    </div>
  )
}
```

---

## shadcndesign.com Pro Blocks

### What Are Pro Blocks?

Pre-built, production-ready component compositions for common page sections:
- Hero sections
- Feature sections
- Testimonials
- Pricing tables
- Product cards
- FAQs
- CTAs

**Key Advantage:** Fully styled, responsive, accessible. Copy-paste and customize.

### Installation

```bash
# Install via CLI (if available in your project)
npx shadcn-ui@latest add <block-name>

# Or manually copy-paste from shadcndesign.com
```

### Common Block Categories

**Marketing Blocks:**
- `hero-01`, `hero-02`, `hero-03` — hero sections with CTA
- `feature-01`, `feature-02` — feature grids
- `testimonial-01`, `testimonial-02` — testimonial sections
- `pricing-01`, `pricing-02` — pricing tables
- `faq-01` — FAQ accordion

**Product Blocks:**
- `product-card-01` — product display card
- `product-filter-01` — product filtering UI
- `product-gallery-01` — image gallery with zoom

**Form Blocks:**
- `contact-form-01` — contact form with validation
- `newsletter-signup-01` — email signup form

**Navigation Blocks:**
- `navbar-01`, `navbar-02` — navigation headers
- `footer-01` — footer with links

### Customization Approach

Pro blocks are fully customizable while keeping system integrity:

1. **Change Colors:** Update Tailwind classes to use your semantic tokens
2. **Change Content:** Replace text, images, links
3. **Change Spacing:** Adjust `gap-` and `p-` classes
4. **Extend Layout:** Add or remove sections

Example: Customizing a hero block

```typescript
// Before (from shadcndesign.com)
export const HeroSection = () => (
  <div className="bg-gradient-to-r from-blue-600 to-blue-800 text-white py-20">
    <h1 className="text-4xl font-bold">Welcome</h1>
    <p className="text-lg mt-4">Start building today</p>
  </div>
)

// After (customized to your brand)
export const HeroSection = () => (
  <div className="bg-gradient-to-r from-primary to-primary/90 text-text-on-primary py-space-2xl">
    <h1 className="text-heading-2xl">Welcome to Our Platform</h1>
    <p className="text-body-md mt-space-lg opacity-90">
      Build, deploy, and scale with ease
    </p>
    <Button appearance="primary" size="lg" className="mt-space-xl">
      Get Started
    </Button>
  </div>
)
```

**Do's:**
- Use your semantic token classes (`bg-primary`, `text-text`, `p-space-lg`)
- Keep responsive breakpoints (`md:text-heading-lg`)
- Maintain accessibility patterns (ARIA labels, semantic HTML)

**Don'ts:**
- Don't hardcode color values (`#3b82f6`)
- Don't break responsive layout (`hidden lg:block`)
- Don't remove accessibility features

### Composition Pattern: Multi-Block Pages

Combine multiple blocks into cohesive pages:

```typescript
// pages/marketing-page.tsx
import { HeroSection } from '@/components/blocks/hero-01'
import { FeatureSection } from '@/components/blocks/feature-01'
import { TestimonialSection } from '@/components/blocks/testimonial-01'
import { PricingSection } from '@/components/blocks/pricing-01'
import { CTASection } from '@/components/blocks/cta-01'

export default function MarketingPage() {
  return (
    <>
      <HeroSection />
      <FeatureSection />
      <TestimonialSection />
      <PricingSection />
      <CTASection />
    </>
  )
}
```

Space blocks consistently:

```typescript
export default function MarketingPage() {
  return (
    <div className="space-y-section">
      <HeroSection />
      <FeatureSection />
      <TestimonialSection />
      <PricingSection />
      <CTASection />
    </div>
  )
}
```

---

## Responsive Composition Strategies

### Mobile-First Breakpoint Strategy

Design for mobile first, enhance upward:

```typescript
// Mobile (320px+): 1 column
// Tablet (768px+): 2 columns
// Desktop (1280px+): 3 columns
export function ProductGrid({ products }: { products: Product[] }) {
  return (
    <div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-space-lg">
      {products.map((product) => (
        <ProductCard key={product.id} product={product} />
      ))}
    </div>
  )
}
```

### Common Responsive Patterns

| Pattern | Mobile | Tablet | Desktop | Classes |
|---------|--------|--------|---------|---------|
| Single Column | 1 col | 1 col | 1 col | `grid-cols-1` |
| Sidebar | Stack | Stack | Side | `flex flex-col lg:flex-row` |
| 2-Column Grid | 1 | 2 | 2 | `grid-cols-1 md:grid-cols-2` |
| 3-Column Grid | 1 | 2 | 3 | `grid-cols-1 md:grid-cols-2 lg:grid-cols-3` |
| Full Width | Full | Full | Container | `w-full lg:max-w-7xl` |

### Hidden/Visible Patterns

```typescript
{/* Show on mobile only */}
<div className="md:hidden">Mobile menu</div>

{/* Show on desktop only */}
<div className="hidden lg:block">Desktop sidebar</div>

{/* Different layouts by breakpoint */}
<div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4">
  {/* Content */}
</div>
```

---

## Component API Design

When creating reusable components, design clear, predictable APIs.

### Prop Patterns

**Appearance/Visual Props:**
```typescript
interface ButtonProps {
  appearance?: 'primary' | 'secondary' | 'ghost'
  size?: 'sm' | 'md' | 'lg'
  width?: 'auto' | 'full'
}
```

**State Props:**
```typescript
interface ButtonProps {
  disabled?: boolean
  loading?: boolean
  selected?: boolean
}
```

**Content Props:**
```typescript
interface ButtonProps {
  children: React.ReactNode
  icon?: React.ReactNode
  rightIcon?: React.ReactNode
}
```

**Event Props:**
```typescript
interface ButtonProps {
  onClick?: (e: React.MouseEvent) => void
  onHover?: (hovered: boolean) => void
}
```

**Composition Props:**
```typescript
interface DialogProps {
  header?: React.ReactNode
  footer?: React.ReactNode
  children: React.ReactNode
}
```

### Controlled vs. Uncontrolled

**Uncontrolled (Recommended):**
Component manages its own state. Simpler for most use cases.

```typescript
// Component
const [value, setValue] = useState('')

return (
  <Input
    value={value}
    onChange={(e) => setValue(e.target.value)}
    defaultValue="initial"
  />
)
```

**Controlled:**
Parent manages state. Use when you need external control.

```typescript
// Parent
const [email, setEmail] = useState('')

// Component
<Input value={email} onChange={(e) => setEmail(e.target.value)} />
```

Document which mode your component supports clearly in Storybook.
