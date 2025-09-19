# Ad Manager Integration Setup Guide

## Overview
This guide explains how to set up the ad manager integration to push user-posted items for targeted ad suggestions.

## Environment Variables

Add these environment variables to your `.env` file:

```env
# Ad Manager Configuration
VITE_AD_MANAGER_API_URL=https://api.admanager.example.com
VITE_AD_MANAGER_API_KEY=your_ad_manager_api_key_here
VITE_AD_MANAGER_ENABLED=false
VITE_AD_MANAGER_PLATFORM=custom
```

## Configuration Options

### Ad Manager Platforms
- `custom` - Your own ad management system
- `google_ads` - Google Ads integration
- `facebook_ads` - Facebook Ads integration

### API Endpoints Required

Your ad manager API should implement these endpoints:

#### 1. Health Check
```
GET /health
Authorization: Bearer {api_key}
```

#### 2. Push Single Item
```
POST /items
Authorization: Bearer {api_key}
X-Platform: {platform}
Content-Type: application/json

{
  "type": "single_item",
  "userId": "user@example.com",
  "item": {
    "id": 123,
    "title": "iPhone 13 Pro",
    "description": "Like new condition",
    "category": "Electronics",
    "brand": "Apple",
    "condition": "Like New",
    "price": 800,
    "location": "New York, NY",
    "author": "user@example.com",
    "date": "2024-01-15",
    "keywords": ["iphone", "13", "pro", "apple", "phone"],
    "tags": ["electronics", "apple", "like new"]
  },
  "timestamp": "2024-01-15T10:30:00Z"
}
```

#### 3. Update Item
```
PUT /items/update
Authorization: Bearer {api_key}
X-Platform: {platform}
Content-Type: application/json

{
  "type": "item_update",
  "userId": "user@example.com",
  "oldItem": { /* previous item data */ },
  "newItem": { /* updated item data */ },
  "timestamp": "2024-01-15T10:30:00Z"
}
```

#### 4. Remove Item
```
DELETE /items/remove
Authorization: Bearer {api_key}
X-Platform: {platform}
Content-Type: application/json

{
  "type": "item_removal",
  "userId": "user@example.com",
  "itemId": 123,
  "timestamp": "2024-01-15T10:30:00Z"
}
```

#### 5. Push User Profile
```
POST /user-profile
Authorization: Bearer {api_key}
X-Platform: {platform}
Content-Type: application/json

{
  "userId": "user@example.com",
  "userEmail": "user@example.com",
  "items": [ /* array of user's items */ ],
  "preferences": {
    "categories": ["Electronics", "Clothing"],
    "priceRange": { "min": 50, "max": 500 },
    "locations": ["New York, NY", "Los Angeles, CA"],
    "brands": ["Apple", "Nike"]
  },
  "lastUpdated": "2024-01-15T10:30:00Z"
}
```

## Data Processing

### Item Categorization
Items are automatically categorized based on:
- **Title keywords** - Extracted from item titles
- **Description keywords** - Extracted from descriptions
- **Category tags** - User-selected categories
- **Brand tags** - Brand information
- **Condition tags** - Item condition

### Privacy Controls
- User consent is required before data is sent
- Personal information is not included in ad targeting data
- Data can be anonymized if configured
- Users can withdraw consent at any time

### Data Retention
- Data is retained for 90 days by default (configurable)
- Users can request data deletion
- Automatic cleanup of old data

## Testing the Integration

### 1. Enable Ad Manager
Set `VITE_AD_MANAGER_ENABLED=true` in your environment variables.

### 2. Test API Connection
The app will automatically check the ad manager health endpoint on startup.

### 3. Test Item Push
1. Log in as a user
2. Go to "My Account" tab
3. Enable "Ad targeting" consent
4. Post a new item
5. Check the ad manager API logs for the item data

### 4. Test Profile Sync
1. With consent enabled, click "Sync My Profile"
2. Check that all user items are sent to the ad manager

## Monitoring and Analytics

### Built-in Tracking
- Item push success/failure rates
- User consent rates
- API response times
- Error logging

### Recommended Metrics
- Number of items pushed per day
- User consent adoption rate
- API error rates
- Ad targeting effectiveness

## Security Considerations

### API Security
- Use HTTPS for all API calls
- Implement proper authentication
- Rate limiting recommended
- Input validation required

### Data Privacy
- Comply with GDPR/CCPA regulations
- Implement data retention policies
- Provide user data export/deletion
- Log all data access

## Troubleshooting

### Common Issues

#### 1. API Connection Failed
- Check `VITE_AD_MANAGER_API_URL` is correct
- Verify API key is valid
- Ensure API server is running

#### 2. Items Not Pushing
- Check user consent is enabled
- Verify API endpoints are implemented
- Check browser console for errors

#### 3. Profile Sync Fails
- Ensure all required endpoints are implemented
- Check API response format
- Verify authentication headers

### Debug Mode
Enable debug logging by adding to your environment:
```env
VITE_DEBUG_AD_MANAGER=true
```

## Next Steps

1. **Set up your ad manager API** with the required endpoints
2. **Configure environment variables** with your API details
3. **Test the integration** with sample data
4. **Monitor performance** and adjust as needed
5. **Implement ad targeting** based on the received data

## Support

For issues or questions:
1. Check the browser console for error messages
2. Verify API endpoint responses
3. Review the configuration settings
4. Test with a simple API client first
