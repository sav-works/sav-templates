# SavTemplates Storefront

A self-hosted, single-page digital product store for **SavTemplates** — premium bilingual (Arabic/English) templates, guides, and tools built for the Gulf/MENA professional market.

Built with pure HTML + CSS + vanilla JavaScript. Zero frameworks. Zero dependencies. One file.

## What's Included

- **6 products** (5 individual + 1 bundle) with full product cards and detail modals
- Hero section with animated stats counter
- Testimonials section
- About / Why choose us section
- FAQ accordion
- Mobile-responsive hamburger menu
- Dark theme with gold accent (#C8A45C)
- Google Fonts: Inter + Noto Kufi Arabic
- Smooth scroll and scroll-triggered animations
- Placeholder "Buy Now" buttons ready for Stripe integration

## How to Deploy (Vercel — Free)

1. **Go to [vercel.com](https://vercel.com)** and sign up (free tier is generous).
2. Click **"Add New" → "Project"**.
3. Drag and drop the entire `store/` folder onto the upload area.
4. Vercel will detect a static site — no configuration needed.
5. Click **"Deploy"**.
6. You'll get a URL like `sav-templates.vercel.app` instantly.

That's it. The store is live in under 60 seconds. Vercel's free tier includes:
- Custom domains
- Automatic HTTPS
- Global CDN
- 100 GB bandwidth/month
- 100 deployments/day

### Alternative: Deploy with any static host
- **Netlify**: Drag the `store/` folder to [netlify.com](https://netlify.com)
- **GitHub Pages**: Push to a repo, enable Pages
- **Cloudflare Pages**: Connect your repo
- **Any web host**: Upload `index.html` to any server

## How to Connect Stripe

Currently, "Buy Now" buttons show "Coming Soon." To add real payments:

### Option A: Stripe Checkout (Simple, ~30 min)

Replace the `onclick="alert(...)"` on each Buy Now button (both in cards and modal) with a redirect to a Stripe Checkout Session.

1. Create a tiny backend (or use Vercel Functions) that calls Stripe's API to create a Checkout Session:

```js
// api/checkout.js (Vercel Function example)
const stripe = require('stripe')(process.env.STRIPE_SECRET_KEY);

module.exports = async (req, res) => {
  const session = await stripe.checkout.sessions.create({
    line_items: [{ price: req.body.priceId, quantity: 1 }],
    mode: 'payment',
    success_url: 'https://yourstore.com/success',
    cancel_url: 'https://yourstore.com/cancel',
  });
  res.json({ url: session.url });
};
```

2. Create price IDs in your [Stripe Dashboard](https://dashboard.stripe.com/products) for each product.
3. Update the HTML buttons to POST to your checkout endpoint and redirect to the returned URL.

### Option B: Stripe Payment Links (Easiest, no code)

1. In Stripe Dashboard → Products → Create a product → Generate a **Payment Link**.
2. Replace the `onclick` on each Buy Now button with `window.location.href = 'https://buy.stripe.com/yourLink'`.

### Option C: Stripe Elements (Custom, advanced)

Embed Stripe Elements directly on a checkout page. This keeps users on your domain for the full purchase flow.

## Cost Comparison: Gumroad vs Self-Hosted

|  | **Gumroad** | **Self-Hosted (Vercel + Stripe)** |
|---|---|---|
| **Monthly fee** | $10/mo (Premium) or $0 + 10% per sale | $0 |
| **Transaction fee** | 9% + $0.30 (free tier) / 4% + $0.30 (Premium) | 2.9% + $0.30 (Stripe only) |
| **On a $29 sale** | Gumroad takes **$2.97** | You keep **$28.72** (Stripe takes $1.14) |
| **On a $79 bundle** | Gumroad takes **$7.41** | You keep **$77.21** (Stripe takes $2.59) |
| **Custom domain** | $10/year (add-on) | Free (Vercel) |
| **Email delivery** | Built-in | Add a free SendGrid or Resend tier |
| **Customer management** | Built-in | Manual or add a free CRM |
| **File hosting** | Included (1 GB for free) | GitHub + CDN (unlimited) |

### Annual savings at 100 sales/month (avg $30)

| Cost | Gumroad (Premium) | Self-Hosted |
|---|---|---|
| Platform fee | $120/yr | $0 |
| Transaction fees | ~$1,800/yr (5% avg after fees) | ~$1,044/yr (2.9% + $0.30) |
| **Total** | **~$1,920/yr** | **~$1,044/yr** |
| **You save** | — | **~$876/year** |

At scale, self-hosting saves thousands annually and gives you full control over your customer experience, branding, and data.

## Customization

1. **Colors**: Edit CSS variables at the top of the `<style>` block in `index.html`
2. **Products**: Edit the `products` array in the `<script>` block
3. **Pricing**: Update `price` and `originalPrice` values in the products array
4. **Images**: Replace gradient placeholders with actual product images by editing the `gradient` property or adding `<img>` tags

## Local Development

No build step needed. Open `index.html` directly in your browser:

```bash
open ~/sav-templates/store/index.html
```

Or serve with any local server:

```bash
npx serve ~/sav-templates/store/
```

## License

© 2026 SavTemplates. All rights reserved.
