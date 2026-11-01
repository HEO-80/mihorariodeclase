# Cambios GSAP ScrollTrigger — Instrucciones para Claude Code

## Dependencias a añadir
Añadir antes del cierre de `</head>` o al final del `<body>`:
```html
<script src="https://cdnjs.cloudflare.com/ajax/libs/gsap/3.12.5/gsap.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/gsap/3.12.5/ScrollTrigger.min.js"></script>
```

## Inicialización
Después de que el DOM cargue (`DOMContentLoaded` o al final del body):
```js
gsap.registerPlugin(ScrollTrigger);
```

## Animaciones a aplicar

### 1. Fade-in + slide-up en secciones
Cada `<section>` aparece suavemente al entrar en viewport:
```js
document.querySelectorAll('section').forEach(sec => {
  gsap.fromTo(sec, { opacity: 0, y: 60 }, {
    opacity: 1, y: 0, duration: 0.9, ease: 'power2.out',
    scrollTrigger: { trigger: sec, start: 'top 85%', toggleActions: 'play none none none' }
  });
});
```

### 2. Stagger en cards (packs, productos, vídeos, etc.)
Cards con clase `.af-glow-card` o equivalente aparecen en cascada:
```js
document.querySelectorAll('.card-container').forEach(parent => {
  const cards = parent.querySelectorAll('.card');
  gsap.fromTo(cards, { opacity: 0, y: 40, scale: 0.95 }, {
    opacity: 1, y: 0, scale: 1, duration: 0.6, stagger: 0.12, ease: 'back.out(1.4)',
    scrollTrigger: { trigger: parent, start: 'top 80%', toggleActions: 'play none none none' }
  });
});
```

### 3. Parallax en imagen hero
La imagen principal del hero se mueve más lento que el scroll:
```js
const heroImg = document.querySelector('.hero-image');
if (heroImg) {
  gsap.to(heroImg, {
    y: 80, ease: 'none',
    scrollTrigger: { trigger: heroImg, start: 'top top', end: 'bottom top', scrub: 0.5 }
  });
}
```

### 4. Scale-in en fotos de competición
Las fotos de "Campeona Bikini Fitness" aparecen con efecto zoom:
```js
document.querySelectorAll('.competition-photo').forEach((img, i) => {
  gsap.fromTo(img, { scale: 0.8, opacity: 0 }, {
    scale: 1, opacity: 1, duration: 0.7, delay: i * 0.1, ease: 'back.out(1.2)',
    scrollTrigger: { trigger: img, start: 'top 85%', toggleActions: 'play none none none' }
  });
});
```

### 5. Títulos h2 se revelan
```js
document.querySelectorAll('h2').forEach(h2 => {
  gsap.fromTo(h2, { opacity: 0, y: 30 }, {
    opacity: 1, y: 0, duration: 0.7, ease: 'power3.out',
    scrollTrigger: { trigger: h2, start: 'top 88%', toggleActions: 'play none none none' }
  });
});
```

### 6. Botones CTA con bounce
Los botones principales (contacto, tienda) aparecen con efecto rebote:
```js
document.querySelectorAll('.cta-button').forEach(btn => {
  gsap.fromTo(btn, { scale: 0.9, opacity: 0 }, {
    scale: 1, opacity: 1, duration: 0.5, ease: 'back.out(2)',
    scrollTrigger: { trigger: btn, start: 'top 90%', toggleActions: 'play none none none' }
  });
});
```

### 7. Parallax en logo brillos (si hay imagen con efecto blend)
```js
const brillos = document.querySelector('.logo-brillos');
if (brillos) {
  gsap.to(brillos, {
    y: -40, ease: 'none',
    scrollTrigger: { trigger: brillos, start: 'top top', end: 'bottom top', scrub: 0.8 }
  });
}
```

## Notas importantes
- Adaptar los selectores CSS (`.card`, `.hero-image`, etc.) a los que use el proyecto real
- `toggleActions: 'play none none none'` = la animación se ejecuta UNA vez al entrar, no se repite al volver arriba
- `scrub` en parallax = la animación sigue el scroll suavemente
- `stagger: 0.12` = 120ms de delay entre cada card
- `ease: 'back.out(1.4)'` = efecto rebote sutil al final
- Todas las animaciones arrancan con los elementos invisibles (`opacity: 0`) — asegurarse de que no hay `opacity: 1` en CSS que compita
