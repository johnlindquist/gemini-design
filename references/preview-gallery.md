# Preview Gallery Templates

After generating variations, create a comparison gallery page. Choose the template matching the project's framework.

## Next.js App Router

Create `app/mockups/page.tsx`:

```tsx
'use client'
import { useState } from 'react'

const variations = [
  { id: '01', name: 'Spacious Editorial', description: 'Typography-led hierarchy with generous whitespace', path: '/mockups/01-spacious-editorial' },
  { id: '02', name: 'Compact Sidebar', description: 'Sidebar navigation, data-dense layout', path: '/mockups/02-compact-sidebar' },
  { id: '03', name: 'Minimal Command Palette', description: 'Command-palette-driven, minimal chrome', path: '/mockups/03-minimal-command-palette' },
]

export default function MockupGallery() {
  const [selected, setSelected] = useState(variations[0].id)
  return (
    <div style={{ fontFamily: 'system-ui', padding: '2rem', maxWidth: '80rem', margin: '0 auto' }}>
      <h1 style={{ fontSize: '1.5rem', fontWeight: 600, marginBottom: '0.5rem' }}>Design Variations</h1>
      <p style={{ color: '#666', marginBottom: '2rem' }}>Click to preview each variation. Open in a new tab for full experience.</p>
      <div style={{ display: 'flex', gap: '1rem', marginBottom: '2rem' }}>
        {variations.map(v => (
          <button key={v.id} onClick={() => setSelected(v.id)}
            style={{ padding: '0.75rem 1.5rem', border: selected === v.id ? '2px solid #0070f3' : '2px solid #e5e5e5',
              borderRadius: '8px', background: selected === v.id ? '#f0f7ff' : '#fff', cursor: 'pointer', textAlign: 'left' }}>
            <div style={{ fontWeight: 600 }}>{v.name}</div>
            <div style={{ fontSize: '0.85rem', color: '#666' }}>{v.description}</div>
          </button>
        ))}
      </div>
      <div style={{ display: 'flex', gap: '1rem' }}>
        <iframe src={variations.find(v => v.id === selected)?.path}
          style={{ width: '100%', height: '80vh', border: '1px solid #e5e5e5', borderRadius: '8px' }} />
      </div>
      <div style={{ marginTop: '1rem', display: 'flex', gap: '0.5rem' }}>
        <a href={variations.find(v => v.id === selected)?.path} target="_blank" rel="noopener"
          style={{ color: '#0070f3', textDecoration: 'none' }}>Open in new tab ↗</a>
      </div>
    </div>
  )
}
```

## Plain HTML

Create `mockups/index.html`:

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Design Variations</title>
  <style>
    * { margin: 0; box-sizing: border-box; }
    body { font-family: system-ui; padding: 2rem; }
    .tabs { display: flex; gap: 0.5rem; margin-bottom: 1rem; }
    .tab { padding: 0.5rem 1rem; border: 2px solid #e5e5e5; border-radius: 6px; cursor: pointer; background: #fff; }
    .tab.active { border-color: #0070f3; background: #f0f7ff; }
    iframe { width: 100%; height: 85vh; border: 1px solid #e5e5e5; border-radius: 8px; }
  </style>
</head>
<body>
  <h1>Design Variations</h1>
  <div class="tabs" id="tabs"></div>
  <iframe id="preview"></iframe>
  <script>
    const variations = [
      { name: 'V1 — Spacious Editorial', src: 'v1.html' },
      { name: 'V2 — Compact Sidebar', src: 'v2.html' },
      { name: 'V3 — Minimal Command Palette', src: 'v3.html' },
    ];
    const tabs = document.getElementById('tabs');
    const preview = document.getElementById('preview');
    variations.forEach((v, i) => {
      const btn = document.createElement('button');
      btn.className = 'tab' + (i === 0 ? ' active' : '');
      btn.textContent = v.name;
      btn.onclick = () => {
        preview.src = v.src;
        document.querySelectorAll('.tab').forEach(t => t.classList.remove('active'));
        btn.classList.add('active');
      };
      tabs.appendChild(btn);
    });
    preview.src = variations[0].src;
  </script>
</body>
</html>
```

## React SPA

Create a preview component and add a route:

```tsx
import { useState } from 'react'
import V1 from './mockups/FeatureV1'
import V2 from './mockups/FeatureV2'
import V3 from './mockups/FeatureV3'

const variations = [
  { name: 'Spacious Editorial', Component: V1 },
  { name: 'Compact Sidebar', Component: V2 },
  { name: 'Minimal Command Palette', Component: V3 },
]

export function MockupGallery() {
  const [idx, setIdx] = useState(0)
  const { Component } = variations[idx]
  return (
    <div>
      <div style={{ display: 'flex', gap: 8, padding: 16, borderBottom: '1px solid #eee' }}>
        {variations.map((v, i) => (
          <button key={i} onClick={() => setIdx(i)}
            style={{ padding: '8px 16px', background: i === idx ? '#0070f3' : '#f5f5f5',
              color: i === idx ? '#fff' : '#333', border: 'none', borderRadius: 6, cursor: 'pointer' }}>
            {v.name}
          </button>
        ))}
      </div>
      <Component />
    </div>
  )
}
```

## Adaptation Notes

- Update the `variations` array to match your actual file paths and names
- The gallery is intentionally plain — it shouldn't impose its own design on the mockups
- For responsive testing, add viewport width controls (320px, 768px, 1024px, 1440px)
- The iframe approach (Next.js/HTML) isolates each variation's styles completely
