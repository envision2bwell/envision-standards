#

## Laravel specific 
Don't use fields such like `lastModifiedDate` or `DateCreated`. Laravel has `created_at` and `updated_at` out of the box. Set model property timestamps = false when not using them
