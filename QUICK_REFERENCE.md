# MediFlow Mobile Optimization - Quick Reference for Developers

## 🚀 Quick Start for Mobile Development

### 1. Mobile-First Breakpoints
Always think smallest screen first, then add larger breakpoints:

```tsx
// ✅ GOOD - Mobile first
<div className="w-full px-4 sm:px-6 md:px-8">
  <h1 className="text-2xl sm:text-3xl md:text-4xl">Title</h1>
</div>

// ❌ BAD - Desktop first
<div className="w-screen px-8">
  <h1 className="text-4xl md:text-3xl">Title</h1>
</div>
```

### 2. Responsive Breakpoints Reference
| Breakpoint | Size | Device | Use Case |
|-----------|------|--------|----------|
| **xs** | 320px | Mobile Portrait | iPhone SE, small phones |
| **sm** | 480px | Mobile Landscape | Landscape phones |
| **md** | 768px | Tablet | iPad, tablets |
| **lg** | 1024px | Desktop | Laptops |
| **xl** | 1280px | Desktop | Large screens |
| **2xl** | 1536px | Wide Desktop | 4K monitors |

### 3. Touch-Friendly Sizing
```tsx
// All buttons and inputs should be minimum 44x44px on mobile
<button className="h-11 px-4 min-h-[44px] min-w-[44px]">
  Click me
</button>

<input 
  type="text" 
  className="h-11 px-4 min-h-[44px]"
  placeholder="Type something"
/>
```

### 4. Responsive Grids
```tsx
// Single column on mobile → 2 columns on tablet → 3 on desktop
<div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-4 md:gap-6">
  {items.map(item => <Card key={item.id} {...item} />)}
</div>
```

### 5. Responsive Padding/Spacing
```tsx
// Mobile-first padding that scales with screen size
<div className="px-4 sm:px-6 md:px-8 py-4 sm:py-6 md:py-8">
  {children}
</div>

// Responsive gaps between elements
<div className="space-y-4 sm:space-y-6 md:space-y-8">
  {children}
</div>
```

### 6. Responsive Typography
```tsx
// Font size scales with viewport
<h1 className="text-2xl sm:text-3xl md:text-4xl lg:text-5xl font-bold">
  Main Title
</h1>

<p className="text-sm sm:text-base md:text-lg text-muted-foreground">
  Body text - always readable
</p>
```

### 7. Responsive Flexbox
```tsx
// Stack on mobile, row on desktop
<div className="flex flex-col md:flex-row gap-4 md:gap-8">
  <div className="flex-1">Left</div>
  <div className="flex-1">Right</div>
</div>
```

### 8. Responsive Navigation
```tsx
// Show mobile menu on small screens, desktop nav on large
<>
  {/* Mobile nav - hidden on md+ */}
  <button className="md:hidden" onClick={toggleMenu}>
    <Menu size={24} />
  </button>

  {/* Desktop nav - hidden on mobile */}
  <nav className="hidden md:flex gap-4">
    <Link to="/home">Home</Link>
    <Link to="/about">About</Link>
  </nav>
</>
```

### 9. Mobile-Only Components
```tsx
// Show only on mobile, hide on desktop
<div className="md:hidden">
  Mobile menu content
</div>

// Show only on desktop, hide on mobile
<div className="hidden md:block">
  Desktop navigation
</div>
```

### 10. Responsive Images
```tsx
// Images scale with container, responsive
<img 
  src="image.webp" 
  alt="Description"
  className="w-full h-auto max-w-2xl"
  loading="lazy"
/>
```

---

## 📋 Common Patterns

### Modal/Dialog (Mobile Bottom Sheet)
```tsx
<div className="
  fixed inset-0 md:inset-auto
  bottom-0 md:top-1/2 md:-translate-y-1/2
  w-full md:w-96
  rounded-t-2xl md:rounded-lg
  p-4 md:p-6
  bg-card
">
  {/* Content */}
</div>
```

### Card with Responsive Padding
```tsx
<div className="
  p-4 sm:p-6 md:p-8
  rounded-lg border border-border
  bg-card
">
  {children}
</div>
```

### Full-Width Button on Mobile
```tsx
<button className="
  w-full md:w-auto
  h-11 md:h-10
  px-4 md:px-6
  py-2 md:py-1
">
  Click me
</button>
```

### Responsive Table → Cards on Mobile
```tsx
<div className="overflow-x-auto">
  {/* Hidden on mobile, shown on md+ */}
  <table className="hidden md:table w-full">
    {/* Desktop table */}
  </table>

  {/* Visible on mobile, hidden on md+ */}
  <div className="md:hidden space-y-3">
    {data.map(item => (
      <Card key={item.id} className="p-4">
        {/* Card view */}
      </Card>
    ))}
  </div>
</div>
```

---

## ⚙️ Component Size Guide

### Button Sizes (Touch-Friendly)
- **icon**: 44x44px (min on mobile) / 40x40px (desktop)
- **sm**: 40px height (mobile) / auto (desktop)
- **default**: 44px height (mobile) / 44px (desktop)
- **lg**: 48px height (mobile) / 48px (desktop)
- **xl**: 56px height (mobile) / 48px (desktop)

### Input Heights
- Standard: 44px on mobile, 40px on desktop
- Compact: 40px on mobile, 36px on desktop
- Large: 48px on mobile, 44px on desktop

### Spacing Units
- Use: 4px, 8px, 12px, 16px, 24px, 32px, 40px, 48px
- Never mix units (e.g., px-4 not px-3 or px-5)
- Use Tailwind's standard spacing scale

---

## 🔍 Testing at Breakpoints

### Mobile Sizes to Test
1. **320px** - iPhone SE (portrait)
2. **375px** - iPhone 12/13 (portrait)
3. **390px** - iPhone 14/15, Pixel (portrait)
4. **480px** - Mobile landscape
5. **768px** - iPad (portrait)
6. **1024px** - iPad landscape / Small desktop
7. **1280px** - Desktop
8. **1920px** - Wide desktop

### Chrome DevTools
1. Press `F12` to open DevTools
2. Click device toggle icon (top-left)
3. Select device or custom size
4. Test at landscape/portrait

---

## ❌ Common Mistakes to Avoid

### 1. Fixed Widths
```tsx
// ❌ BAD - breaks on mobile
<div style={{ width: '960px' }} />

// ✅ GOOD - responsive
<div className="w-full max-w-6xl" />
```

### 2. Fixed Font Sizes
```tsx
// ❌ BAD - too small on mobile
<p className="text-xs">Body text</p>

// ✅ GOOD - scales with device
<p className="text-sm md:text-base">Body text</p>
```

### 3. Touch Targets Too Small
```tsx
// ❌ BAD - 24x24px too small
<button className="p-1">x</button>

// ✅ GOOD - 44x44px minimum
<button className="p-3 h-11 w-11 min-h-[44px]">x</button>
```

### 4. No Responsive Padding
```tsx
// ❌ BAD - same padding everywhere
<div className="p-8">
  {children}
</div>

// ✅ GOOD - scales with screen
<div className="p-4 md:p-8">
  {children}
</div>
```

### 5. Hidden Overflow Issues
```tsx
// ❌ BAD - content cuts off on mobile
<div className="overflow-hidden">
  <table className="w-full" />
</div>

// ✅ GOOD - horizontal scroll on mobile if needed
<div className="overflow-x-auto md:overflow-visible">
  <table className="w-full" />
</div>
```

### 6. Hover-Only Interactions
```tsx
// ❌ BAD - doesn't work on touch
<div className="hidden hover:block">Menu</div>

// ✅ GOOD - works on touch and hover
<button className="block focus-visible:ring-2" onClick={toggle}>
  Menu
</button>
```

### 7. Invisible Focus States
```tsx
// ❌ BAD - no focus indicator for keyboard users
<button>Click me</button>

// ✅ GOOD - visible focus state
<button className="focus-visible:ring-2 focus-visible:ring-primary">
  Click me
</button>
```

---

## 🎨 Tailwind Classes You'll Use Often

### Responsive Display
```tsx
className="block md:hidden"  // Hidden on desktop
className="hidden md:block"  // Hidden on mobile
className="flex md:grid"     // Different layouts
```

### Responsive Sizing
```tsx
className="w-full md:w-96"   // Full width on mobile, 384px on desktop
className="h-11 md:h-10"     // 44px on mobile, 40px on desktop
className="text-2xl md:text-4xl"
```

### Responsive Spacing
```tsx
className="px-4 md:px-8"     // 16px on mobile, 32px on desktop
className="py-4 md:py-8"
className="gap-4 md:gap-8"
className="space-y-4 md:space-y-8"
```

### Responsive Grid
```tsx
className="grid-cols-1 md:grid-cols-2 lg:grid-cols-3"
className="gap-4 md:gap-6 lg:gap-8"
```

---

## 📊 Performance Considerations

### For Mobile Optimization
1. **Use webp images** - Smaller file size
2. **Enable lazy loading** - `loading="lazy"` on images
3. **Code split routes** - `React.lazy()` for heavy pages
4. **Minimize animations** - Respect `prefers-reduced-motion`
5. **Cache aggressively** - Service Worker for offline
6. **Bundle splitting** - Keep main bundle < 100KB

### Lighthouse Target Scores (Mobile)
- **Performance**: > 90
- **Accessibility**: > 95
- **Best Practices**: > 90
- **SEO**: > 95

---

## 🔗 Resources & Documentation

### Tailwind CSS
- [Responsive Design](https://tailwindcss.com/docs/responsive-design)
- [Breakpoints](https://tailwindcss.com/docs/breakpoints)
- [Width Utilities](https://tailwindcss.com/docs/width)

### Mobile Web Best Practices
- [MDN: Responsive Design](https://developer.mozilla.org/en-US/docs/Learn/CSS/CSS_layout/Responsive_Design)
- [Google Mobile-Friendly](https://search.google.com/test/mobile-friendly)
- [WCAG 2.1 Mobile](https://www.w3.org/WAI/mobile/)

### Our Documentation
- [MOBILE_OPTIMIZATION_GUIDE.md](./MOBILE_OPTIMIZATION_GUIDE.md) - Comprehensive guide
- [IMPLEMENTATION_CHECKLIST.md](./IMPLEMENTATION_CHECKLIST.md) - Tasks and testing

---

## 💡 Tips & Tricks

1. **Always test on real devices** - DevTools emulation isn't perfect
2. **Use DevTools throttling** - Test on Slow 4G to catch performance issues
3. **Check orientation** - Portrait AND landscape
4. **Test with zoom** - Try 150%, 200% zoom levels
5. **Keyboard navigation** - Tab through all interactive elements
6. **Screen readers** - Test with VoiceOver (iOS) or TalkBack (Android)
7. **Use `clamp()`** - For truly fluid, responsive sizing
8. **Minimize JavaScript** - Mobile devices have less CPU power
9. **Preload critical resources** - Images, fonts above the fold
10. **Monitor real-world metrics** - Use Core Web Vitals

---

## 🎯 Implementation Workflow

### When Building a New Feature
1. **Start with mobile layout** (single column, stacked)
2. **Add tablet enhancements** at `md:` breakpoint (2 columns)
3. **Add desktop enhancements** at `lg:` breakpoint (3+ columns)
4. **Test at all breakpoints** with DevTools
5. **Test on real device** (iOS and Android)
6. **Optimize images** for different sizes
7. **Check Lighthouse score** (> 90 on mobile)
8. **Deploy and monitor** - Watch for issues

---

Need help? Check:
- [Component documentation](./src/components/ui/)
- [Example pages](./src/pages/)
- [Tailwind docs](https://tailwindcss.com/)
