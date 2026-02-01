# CSS Improvements Reference

## Animation Patterns (Matching svm_methods.html)

### Card Entrance Animation
```css
.limitation-card {
    animation: slideUpCard 0.7s ease-out forwards;
}

.limitation-card:nth-child(n) { 
    animation-delay: 0.1s * n;  /* Cascading stagger effect */
}

@keyframes slideUpCard {
    from { opacity: 0; transform: translateY(30px); }
    to { opacity: 1; transform: translateY(0); }
}
```

### Hover Transformation
```css
.limitation-card:hover {
    transform: translateY(-12px) scale(1.02);
    box-shadow: 0 12px 35px rgba(255, 76, 76, 0.2);
    border-color: var(--red);
}
```

### Icon Animation
```css
.limitation-icon {
    transition: transform 0.4s cubic-bezier(0.34, 1.56, 0.64, 1);
}

.limitation-card:hover .limitation-icon {
    transform: scale(1.2) rotate(8deg);
}
```

---

## Color Gradient System

### 1. Red → Yellow (Limitation Cards)
**Primary Use**: 8 main limitation cards at top of page

```css
background: linear-gradient(135deg, 
    rgba(255, 76, 76, 0.1),      /* Red base */
    rgba(255, 235, 59, 0.1)      /* Yellow fade */
);

/* Hover state intensification */
background: linear-gradient(135deg, 
    rgba(255, 76, 76, 0.15),     /* Darker red */
    rgba(255, 235, 59, 0.15)     /* Darker yellow */
);
```

**Icon Design**:
```css
.limitation-icon {
    background: linear-gradient(135deg, 
        var(--red),               /* #ff4c4c */
        #ff6666                   /* Lighter red */
    );
}
```

---

### 2. Purple → Pink (Key Considerations)
**Primary Use**: "Key Considerations for Model Interpretation" section

```css
.key-considerations {
    background: linear-gradient(135deg, 
        rgba(128, 0, 192, 0.08),  /* Purple */
        rgba(255, 64, 128, 0.08)  /* Pink */
    );
    border: 3px solid rgb(var(--light-gray));
    border-radius: 50px;
}

.consideration-item {
    background: linear-gradient(135deg, 
        rgba(200, 128, 224, 0.08),    /* Light purple */
        rgba(255, 128, 176, 0.08)     /* Light pink */
    );
}
```

**Bullet Design**:
```css
.consideration-bullet {
    background: linear-gradient(135deg, 
        #8000c0,      /* Purple */
        #ff4080       /* Hot pink */
    );
}
```

---

### 3. Blue → Green (Statistical Note)
**Primary Use**: "Our Data in Context" statistics callout

```css
.statistical-note {
    background: linear-gradient(135deg, 
        rgba(30, 144, 255, 0.12),      /* Blue */
        rgba(40, 167, 69, 0.12)        /* Green */
    );
}

.stat-icon {
    background: linear-gradient(135deg, 
        var(--blue),       /* #1e90ff */
        #28a745            /* Green */
    );
}
```

---

### 4. Giant Rainbow Gradient (Body Background)
**Primary Use**: Page background (from svm_methods.html)

```css
body {
    background: linear-gradient(
        135deg,
        rgba(255, 245, 245, 0.75) 0%,      /* Red-tinted white */
        rgba(255, 252, 240, 0.75) 16.67%,  /* Yellow-tinted white */
        rgba(245, 255, 245, 0.75) 33.33%,  /* Green-tinted white */
        rgba(240, 250, 255, 0.75) 50%,     /* Blue-tinted white */
        rgba(250, 245, 255, 0.75) 66.67%,  /* Purple-tinted white */
        rgba(255, 245, 250, 0.75) 100%     /* Pink-tinted white */
    );
    background-attachment: fixed;
}
```

---

## Responsive Breakpoints

### Desktop (1200px+)
```css
.limitations-grid {
    grid-template-columns: repeat(auto-fit, minmax(320px, 1fr));
    gap: clamp(2rem, 4vw, 3rem);
    padding: clamp(3rem, 5vw, 4rem);
}
```

### Tablet (768px - 1200px)
```css
@media (max-width: 900px) {
    .considerations-list {
        grid-template-columns: 1fr;
    }
    
    .consideration-bullet {
        width: 38px;
        height: 38px;
    }
}
```

### Mobile (< 768px)
```css
@media (max-width: 768px) {
    .limitation-card {
        padding: clamp(1.5rem, 3vw, 2rem);
    }
    
    .consideration-item {
        gap: 1rem;
        padding: clamp(1rem, 2vw, 1.5rem);
    }
}
```

---

## Animation Timing

All animations use the same cubic-bezier for consistency:

```css
cubic-bezier(0.34, 1.56, 0.64, 1)  /* Smooth bounce effect */
```

This creates a subtle "spring" effect that feels modern and professional while maintaining readability.

### Staggered Entrance Times
- **Limitation Cards**: 0.1s to 0.8s (7 steps × 0.1s)
- **Statistical Note**: 0.9s (coordinated after cards)
- **Consideration Items**: 1.0s to 1.4s (5 steps × 0.1s)

This creates a fluid cascade effect when the page loads.

---

## Enhanced Hover States

```css
/* Cards lift and scale on hover */
transform: translateY(-12px) scale(1.02);
box-shadow: 0 12px 35px rgba(color, 0.2);

/* Icons rotate playfully */
transform: scale(1.2) rotate(8deg);

/* Considerations highlight with color shift */
border-color: #8000c0;
background: linear-gradient(135deg, 
    rgba(200, 128, 224, 0.12),
    rgba(255, 128, 176, 0.12)
);
```

---

## Key CSS Variables Used

```css
--red: #ff4c4c
--blue: #1e90ff
--green: #28a745
--yellow: #ffeb3b
--black: #000000
--light-gray: 221, 221, 221

/* Custom for limitations */
#8000c0 (Purple)
#ff4080 (Hot Pink)
#5dade2 (Light Blue)
#ff6666 (Light Red)
```

---

## Browser Compatibility

All animations and gradients use standard CSS3 properties with excellent browser support:

- ✅ Firefox 16+
- ✅ Chrome/Edge 26+
- ✅ Safari 6.1+
- ✅ iOS Safari 7+
- ✅ Android 4.4+

No vendor prefixes required for the animations used.
