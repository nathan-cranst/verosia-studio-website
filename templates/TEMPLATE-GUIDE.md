# Verosia Studio Template System

This folder contains reusable templates for creating new pages with full functionality including analytics, cookies, dark mode, and responsive design.

## Template Files

### 1. `complete-page-template.html`
**Complete page template** - Copy this entire file and rename it for new pages. Includes everything you need.

### 2. `contact-form-template.html`
**Contact form template** - Reusable contact form with validation, service selection, and Netlify integration.

### 3. `header.html`
**Header template** - Navigation, dropdowns, CTAs, and cookie consent banner.

### 4. `footer.html`
**Footer template** - Links, contact info, social media, and copyright.

## How to Use

### Creating New Pages (Recommended)
1. Copy `complete-page-template.html`
2. Rename to your new page (e.g., `new-service.html`)
3. Update the title and meta description
4. Add your content in the main section
5. Done!

### Adding Contact Forms
- Use `contact-form-template.html` as a reference for form structure and validation
- Copy the form HTML and customize fields as needed

## What's Included

✅ **Google Tag Manager** - Full analytics tracking
✅ **Cookie Consent Banner** - GDPR compliant
✅ **Dark Mode Toggle** - With localStorage persistence
✅ **Responsive Navigation** - Mobile-friendly
✅ **Service Dropdowns** - With images and CTAs
✅ **Blog Dropdown** - With featured posts
✅ **CTA Buttons** - "Free Digital Audit" and "Ask us anything"
✅ **Social Media Links** - With proper security attributes
✅ **Contact Information** - Phone, email, hours
✅ **Legal Links** - Terms, FAQ, etc.
✅ **Performance Optimized** - Lazy loading, minified assets
✅ **Accessibility** - Alt text, ARIA labels, semantic HTML

## Maintenance

- **Update once, apply everywhere** - Changes to `header.html` and `footer.html` automatically apply to all pages
- **Analytics tracking** - All pages automatically track user interactions
- **Cookie compliance** - GDPR banner appears on all pages
- **Consistent branding** - Logo, colors, and styling maintained across all pages

## File Structure

```
templates/
├── complete-page-template.html    # Full page template
├── contact-form-template.html     # Contact form
├── header.html                   # Header with nav
├── footer.html                   # Footer content
└── TEMPLATE-GUIDE.md             # This file
```

## Notes

- Always use `pt-8 pb-5` for main content sections to account for sticky header
- Update relative paths if creating pages in subfolders
- Test on mobile devices after creating new pages
- Keep templates updated for consistent functionality


## Cookie Banner Implementation

### For Pages Using `loadTemplate()` Function
If your page uses the `loadTemplate()` function (like most existing pages), add this cookie banner initialization function:

```javascript
// Cookie Banner Initialization
function initializeCookieBanner() {
    window.dataLayer = window.dataLayer || [];
    function gtag(){ dataLayer.push(arguments); }

    // Show banner unless a previous choice exists
    var choice = localStorage.getItem('vs_consent');
    if (!choice) {
        var banner = document.getElementById('vs-consent');
        if (banner) banner.style.display = 'block';
    }

    function setConsent(state){
        gtag('consent','update',{
            'ad_storage': state,
            'analytics_storage': state,
            'ad_user_data': state,
            'ad_personalization': state
        });
        localStorage.setItem('vs_consent', state);
        var banner = document.getElementById('vs-consent');
        if (banner) banner.style.display = 'none';
    }

    // Add event listeners
    var acceptBtn = document.getElementById('vs-accept');
    var rejectBtn = document.getElementById('vs-reject');
    
    if (acceptBtn) acceptBtn.onclick = function(){ setConsent('granted'); };
    if (rejectBtn) rejectBtn.onclick = function(){ setConsent('denied'); };

    // Optional: provide a way to re-open banner
    window.openConsent = function(){
        localStorage.removeItem('vs_consent');
        var banner = document.getElementById('vs-consent');
        if (banner) banner.style.display = 'block';
    };
}
```

**Then update your `loadTemplate()` function to call it:**
```javascript
// Simple template loader
async function loadTemplate(containerId, templatePath) {
    try {
        const response = await fetch(templatePath);
        if (!response.ok) {
            throw new Error(`HTTP error! status: ${response.status}`);
        }
        const html = await response.text();
        document.getElementById(containerId).innerHTML = html;

        console.log(`✅ ${templatePath} loaded successfully`);
        
        // Initialize cookie banner after header loads
        if (templatePath === 'templates/header.html') {
            initializeCookieBanner();
        }
    } catch (error) {
        console.error(`❌ Error loading ${templatePath}:`, error);
    }
}
```

### For Pages Using Direct `fetch()` (New Template System)
Use the complete template loading script below.

## Code Snippets for Template Integration

### For New Pages (Complete Template)
Use `complete-page-template.html` - it includes everything you need.

### For Existing Pages (Add Template Functionality)

#### 1. Add to `<head>` section:
```html
<!-- Google Tag Manager -->
<script>(function(w,d,s,l,i){w[l]=w[l]||[];w[l].push({'gtm.start':
new Date().getTime(),event:'gtm.js'});var f=d.getElementsByTagName(s)[0],
j=d.createElement(s),dl=l!='dataLayer'?'&l='+l:'';j.async=true;j.src=
'https://www.googletagmanager.com/gtm.js?id='+i+dl;f.parentNode.insertBefore(j,f);
})(window,document,'script','dataLayer','GTM-K5QRF4CR');</script>
<!-- End Google Tag Manager -->

<!-- Dark mode script -->
<script>
const storedTheme = localStorage.getItem('theme')
const getPreferredTheme = () => {
    if (storedTheme) {
        return storedTheme
    }
    return window.matchMedia('(prefers-color-scheme: dark)').matches ? 'dark' : 'light'
}
const setTheme = function (theme) {
    if (theme === 'auto' && window.matchMedia('(prefers-color-scheme: dark)').matches) {
        document.documentElement.setAttribute('data-bs-theme', 'dark')
    } else {
        document.documentElement.setAttribute('data-bs-theme', theme)
    }
}
setTheme(getPreferredTheme())
// ... rest of dark mode script
</script>

<!-- CSS Links -->
<link href="assets/vendor/bootstrap/css/bootstrap.min.css" rel="stylesheet"> 
<link href="assets/vendor/bootstrap-icons/bootstrap-icons.css" rel="stylesheet"> 
<link href="assets/vendor/aos/aos.css" rel="stylesheet"> 
<link href="assets/vendor/glightbox/css/glightbox.css" rel="stylesheet"> 
<link href="assets/vendor/swiper/swiper-bundle.min.css" rel="stylesheet"> 
<link rel="stylesheet" type="text/css" href="assets/vendor/font-awesome/css/all.min.css"> 
<link rel="stylesheet" type="text/css" href="assets/css/style.css">
```

#### 2. Add after `<body>` tag:
```html
<!-- Google Tag Manager (noscript) -->
<noscript><iframe src="https://www.googletagmanager.com/ns.html?id=GTM-K5QRF4CR"
height="0" width="0" style="display:none;visibility:hidden"></iframe></noscript>
<!-- End Google Tag Manager (noscript) -->

<!-- Header Container -->
<div id="header-container"></div>
```

#### 3. Add before `</body>` tag:
```html
<!-- Footer Container -->
<div id="footer-container"></div>

<!-- Back to top -->         
<div class="back-top"><i class="bi bi-arrow-up-short position-absolute top-50 start-50 translate-middle"></i></div>

<!-- Scripts -->
<script src="assets/vendor/bootstrap/dist/js/bootstrap.bundle.min.js"></script> 
<script src="assets/vendor/purecounterjs/dist/purecounter_vanilla.js"></script> 
<script src="assets/vendor/aos/aos.js"></script> 
<script src="assets/vendor/glightbox/js/glightbox.js"></script> 
<script src="assets/vendor/swiper/swiper-bundle.min.js"></script> 
<script src="assets/js/functions.js"></script> 

<!-- Load Header and Footer Templates -->
<script>
    // Load header
    fetch('templates/header.html')
        .then(response => response.text())
        .then(data => {
            document.getElementById('header-container').innerHTML = data;
            // Initialize cookie banner after header loads
            initializeCookieBanner();
        })
        .catch(error => console.error('Error loading header:', error));

    // Load footer
    fetch('templates/footer.html')
        .then(response => response.text())
        .then(data => {
            document.getElementById('footer-container').innerHTML = data;
        })
        .catch(error => console.error('Error loading footer:', error));

    // Cookie Banner Initialization
    function initializeCookieBanner() {
        window.dataLayer = window.dataLayer || [];
        function gtag(){ dataLayer.push(arguments); }

        // Show banner unless a previous choice exists
        var choice = localStorage.getItem('vs_consent');
        if (!choice) {
            var banner = document.getElementById('vs-consent');
            if (banner) banner.style.display = 'block';
        }

        function setConsent(state){
            gtag('consent','update',{
                'ad_storage': state,
                'analytics_storage': state,
                'ad_user_data': state,
                'ad_personalization': state
            });
            localStorage.setItem('vs_consent', state);
            var banner = document.getElementById('vs-consent');
            if (banner) banner.style.display = 'none';
        }

        // Add event listeners
        var acceptBtn = document.getElementById('vs-accept');
        var rejectBtn = document.getElementById('vs-reject');
        
        if (acceptBtn) acceptBtn.onclick = function(){ setConsent('granted'); };
        if (rejectBtn) rejectBtn.onclick = function(){ setConsent('denied'); };

        // Optional: provide a way to re-open banner
        window.openConsent = function(){
            localStorage.removeItem('vs_consent');
            var banner = document.getElementById('vs-consent');
            if (banner) banner.style.display = 'block';
        };
    }
</script>
```