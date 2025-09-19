# Admin Guide - Deleted Accounts Management

## Overview
When users delete their accounts, the data is preserved for admin review instead of being permanently deleted. This allows you to maintain data integrity and handle any disputes or issues.

## How Account Deletion Works

### User Experience:
1. User clicks "Delete Account" in their account settings
2. User must type "DELETE" to confirm
3. User sees warning about data retention
4. Account is marked as deleted but data is preserved
5. User is signed out and cannot log back in

### Admin Access:
- All deleted account data remains in Supabase
- Data is marked with deletion metadata
- You can query and manage deleted accounts

## Querying Deleted Accounts in Supabase

### 1. View All Deleted Users
```sql
SELECT 
  id,
  email,
  user_metadata->>'name' as name,
  user_metadata->>'deleted' as is_deleted,
  user_metadata->>'deleted_at' as deleted_at,
  user_metadata->>'deleted_reason' as deletion_reason,
  created_at
FROM auth.users 
WHERE user_metadata->>'deleted' = 'true'
ORDER BY user_metadata->>'deleted_at' DESC;
```

### 2. View Items from Deleted Users
```sql
SELECT 
  i.*,
  u.email as user_email,
  u.user_metadata->>'name' as user_name,
  u.user_metadata->>'deleted_at' as account_deleted_at
FROM items i
JOIN auth.users u ON i.author = u.email
WHERE u.user_metadata->>'deleted' = 'true'
ORDER BY u.user_metadata->>'deleted_at' DESC;
```

### 3. View Offers from Deleted Users
```sql
SELECT 
  o.*,
  u.email as user_email,
  u.user_metadata->>'name' as user_name,
  u.user_metadata->>'deleted_at' as account_deleted_at
FROM offers o
JOIN auth.users u ON o.author = u.email
WHERE u.user_metadata->>'deleted' = 'true'
ORDER BY u.user_metadata->>'deleted_at' DESC;
```

## Admin Actions

### 1. Restore a Deleted Account
```sql
UPDATE auth.users 
SET user_metadata = user_metadata - 'deleted' - 'deleted_at' - 'deleted_reason'
WHERE id = 'USER_ID_HERE';
```

### 2. Permanently Delete Account Data
```sql
-- Delete user's items
DELETE FROM items WHERE author = 'USER_EMAIL_HERE';

-- Delete user's offers
DELETE FROM offers WHERE author = 'USER_EMAIL_HERE';

-- Delete the user account
DELETE FROM auth.users WHERE id = 'USER_ID_HERE';
```

### 3. Export Deleted Account Data
```sql
-- Export all data for a specific deleted user
SELECT 
  'user' as data_type,
  id,
  email,
  user_metadata,
  created_at
FROM auth.users 
WHERE id = 'USER_ID_HERE'

UNION ALL

SELECT 
  'item' as data_type,
  id::text,
  title,
  description,
  created_at
FROM items 
WHERE author = 'USER_EMAIL_HERE'

UNION ALL

SELECT 
  'offer' as data_type,
  id::text,
  message,
  offer_price,
  created_at
FROM offers 
WHERE author = 'USER_EMAIL_HERE';
```

## Data Retention Policy

### Recommended Approach:
1. **Keep deleted data for 30-90 days** for dispute resolution
2. **Anonymize data** after retention period (remove personal info)
3. **Archive important data** before permanent deletion
4. **Log all admin actions** for audit purposes

### Data Cleanup Schedule:
- **Weekly**: Review new deletions
- **Monthly**: Anonymize old deleted accounts
- **Quarterly**: Permanent deletion of anonymized data

## Monitoring Deleted Accounts

### Key Metrics to Track:
- Number of accounts deleted per day/week/month
- Reasons for deletion (if collected)
- Data volume from deleted accounts
- Disputes or issues requiring data access

### Alerts to Set Up:
- Unusual spike in account deletions
- Large data volumes from single user
- Failed deletion attempts

## Security Considerations

### Access Control:
- Limit admin access to trusted personnel only
- Use Supabase RLS policies for additional security
- Log all admin queries and actions
- Regular access reviews

### Data Privacy:
- Ensure compliance with GDPR/CCPA
- Implement data anonymization procedures
- Regular data retention audits
- Clear data handling policies

## Troubleshooting

### Common Issues:
1. **User can't delete account**: Check Supabase permissions
2. **Data not marked as deleted**: Verify user_metadata update
3. **Can't query deleted data**: Check RLS policies
4. **Performance issues**: Add indexes on deletion fields

### Support Queries:
- User claims account was deleted by mistake
- Legal requests for user data
- Data export requests
- Account restoration requests

## Best Practices

1. **Document all procedures** for team members
2. **Test deletion process** regularly
3. **Monitor system performance** with large datasets
4. **Keep backups** before permanent deletion
5. **Regular security audits** of admin access
6. **Clear communication** with users about data retention

## Contact Information

For technical issues or questions about this system:
- **Admin Email**: [Your admin email]
- **Supabase Project**: [Your project URL]
- **Documentation**: [Link to this guide]

