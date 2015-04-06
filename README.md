# Top GitHub users

# License

Licensed under the Public Domain.


# Using

Thanks to @redox for the [starter query.](https://github.com/redox/top-github-users/)

This repository lists the top users of github during the last 5 years. It has been built using the following BigQuery

```sql
SELECT
  a.actor_attributes_login as login,
  a.actor_attributes_name as name,
  a.actor_attributes_company as company,
  a.actor_attributes_location as location,
  a.actor_attributes_blog AS blog,
  a.actor_attributes_email AS email,
  b.event_count AS event_count
FROM [githubarchive:github.timeline] a
JOIN EACH
  (
     SELECT MAX(created_at) as max_created, actor_attributes_login, COUNT(*) AS event_count
     FROM [githubarchive:github.timeline]
     GROUP EACH BY actor_attributes_login
  ) b
  ON 
  b.max_created = a.created_at and
  b.actor_attributes_login = a.actor_attributes_login
WHERE a.actor_attributes_name IS NOT NULL OR a.actor_attributes_email IS NOT NULL OR a.actor_attributes_blog IS NOT NULL
ORDER BY event_count DESC
LIMIT 1000000;
```

