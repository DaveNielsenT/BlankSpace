# Google AdSense Setup Instructions

## 1. Get Your AdSense Account

1. **Apply for AdSense** at [adsense.google.com](https://adsense.google.com)
2. **Get approved** (may take a few days to weeks)
3. **Get your Publisher ID** (starts with `ca-pub-`)

## 2. Configure Your AdSense Settings

### Update `src/config/adsense.ts`:
```typescript
export const ADSENSE_CONFIG = {
  // Replace with your actual AdSense Publisher ID
  CLIENT_ID: 'ca-pub-YOUR_ACTUAL_PUBLISHER_ID',
  
  // Replace with your actual ad slot IDs
  AD_SLOTS: {
    BANNER_BOTTOM: 'YOUR_ACTUAL_BANNER_AD_SLOT_ID',
    SIDEBAR: 'YOUR_ACTUAL_SIDEBAR_AD_SLOT_ID',
    IN_ARTICLE: 'YOUR_ACTUAL_IN_ARTICLE_AD_SLOT_ID',
    MOBILE_BANNER: 'YOUR_ACTUAL_MOBILE_BANNER_AD_SLOT_ID'
  },
  // ... rest of config
}
```

### Update `index.html`:
```html
<script async src="https://pagead2.googlesyndication.com/pagead/js/adsbygoogle.js?client=ca-pub-YOUR_ACTUAL_PUBLISHER_ID"
     crossorigin="anonymous"></script>
```

## 3. Create Ad Units in AdSense

1. **Go to AdSense Dashboard**
2. **Click "Ads" → "By ad unit"**
3. **Create new ad units**:
   - **Banner Ad** (728x90) for bottom of page
   - **Mobile Banner** (320x50) for mobile
   - **Rectangle** (300x250) for sidebar
   - **In-Article** for content areas

4. **Copy the ad slot IDs** and update the config file

## 4. Monetization Tracking

The app includes built-in tracking for:
- **Ad impressions** - when ads are displayed
- **Ad clicks** - when users click ads
- **Revenue tracking** - for revenue analytics

### Analytics Integration:
```typescript
// In src/config/adsense.ts
export const MONETIZATION_TRACKING = {
  trackImpression: (adSlot: string) => {
    // Add Google Analytics tracking
    gtag('event', 'ad_impression', { ad_slot: adSlot })
  },
  
  trackClick: (adSlot: string) => {
    // Add Google Analytics tracking
    gtag('event', 'ad_click', { ad_slot: adSlot })
  },
  
  trackRevenue: (amount: number, currency: string = 'USD') => {
    // Add revenue tracking
    gtag('event', 'purchase', { value: amount, currency: currency })
  }
}
```

## 5. Ad Placement Strategy

### Current Implementation:
- **Bottom Banner** - 728x90 banner at bottom of page
- **Responsive** - automatically adjusts for mobile
- **Non-intrusive** - doesn't interfere with user experience

### Future Ad Placements:
- **Sidebar ads** for desktop
- **In-article ads** within content
- **Mobile-specific** banner ads
- **Sponsored content** integration

## 6. Revenue Optimization

### Best Practices:
1. **Place ads** where users naturally look
2. **Don't overdo it** - too many ads hurt user experience
3. **Test different placements** to find what works
4. **Monitor performance** in AdSense dashboard
5. **A/B test** different ad sizes and formats

### Tracking Metrics:
- **CTR (Click-Through Rate)** - percentage of users who click ads
- **RPM (Revenue Per Mille)** - revenue per 1000 page views
- **CPC (Cost Per Click)** - how much you earn per click
- **Fill Rate** - percentage of ad requests that show ads

## 7. Compliance

### AdSense Policies:
- **Don't click your own ads**
- **Don't ask others to click ads**
- **Follow content policies**
- **Respect user experience**
- **Don't place ads on prohibited content**

### Privacy:
- **GDPR compliance** for EU users
- **Cookie consent** if required
- **User privacy** protection

## 8. Testing

### Before Going Live:
1. **Test in development** with test ads
2. **Check mobile responsiveness**
3. **Verify ad loading** works correctly
4. **Test tracking** functionality
5. **Check page load speed**

### Production Checklist:
- [ ] AdSense account approved
- [ ] Publisher ID configured
- [ ] Ad slot IDs configured
- [ ] Ads loading correctly
- [ ] Tracking working
- [ ] Mobile responsive
- [ ] No console errors

## 9. Monitoring

### AdSense Dashboard:
- **Performance** metrics
- **Revenue** tracking
- **Ad unit** performance
- **Geographic** data
- **Device** breakdown

### Analytics Integration:
- **Google Analytics** for detailed tracking
- **Custom events** for ad interactions
- **Revenue** attribution
- **User behavior** analysis

## 10. Troubleshooting

### Common Issues:
1. **Ads not showing** - check ad slot IDs
2. **Low fill rate** - may need more traffic
3. **Policy violations** - check AdSense policies
4. **Slow loading** - optimize ad placement
5. **Mobile issues** - check responsive design

### Support:
- **AdSense Help Center**
- **Google AdSense Community**
- **AdSense Support** (for account issues)

