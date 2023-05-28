# Instagram User Analytics

### Project Description: 
> In this project, I have analyzed user data from Instagram to provide insights to the marketing and investor teams. The aim is to derive business insights that can be used to launch new marketing campaigns, decide on features to build for the app, track user engagement and improve the overall experience of the app to help the business grow.

### Tech-Stack Used: 
> For this project, I have used Microsoft SQL sever and PopSQL as query editor to run SQL queries and analyze the database provided with visualization.

### Insights: 
> Through this project, I gained clear understanding of users and there behavior on Instagram, moreover got to know how the platform can be used for marketing and growth with collections every individual.

<br>

## A) Marketing: 
The marketing team wants to launch some campaigns, here are solutions for below asked 


### 1. Rewarding Most Loyal Users:
---
People who have been using the platform for the longest time.

> **Task**: Find the 5 oldest users of the Instagram from the database provided

> **Approach**: 

```
select top(5) * from users order by created_at asc
```

> **Results**: Given below is a list of 5 oldest users of Instagram 

| id  | username         | created_at          |
| --- | ---------------- | ------------------- |
| 80  | Darby_Herzog     | 2016-05-06 00:14:21 |
| 67  | Emilio_Bernier52 | 2016-05-06 13:04:30 |
| 63  | Elenor88         | 2016-05-08 01:30:41 |
| 95  | Nicole71         | 2016-05-09 17:30:22 |
| 38  | Jordyn.Jacobson2 | 2016-05-14 07:56:26 |

<br>

### 2. Remind Inactive Users to Start Posting
---
By sending them promotional emails to post their 1st photo.

> **Task**: Find the users who have never posted a single photo on Instagram

> **Approach**: 

```
SELECT id, username FROM users WHERE id NOT IN (SELECT DISTINCT user_id FROM photos);
```

> This query selects the id and username columns from the users table. It then uses a subquery to retrieve the distinct user_id values from the photos table. The main query filters out the users whose id values are not present in the subquery result, indicating that they have never posted a photo.

```
select u.id, u.username, u.created_at from users u left join   photos p on u.id = p.user_id where p.user_id is null
```

> This query uses a LEFT JOIN between the users and photos tables and then filters out the rows where p.user_id is NULL. This condition ensures that only the users who have never posted a single photo are returned.

> **Results**: Given below is a list of 26 users, who never posted a photo to instagram

| id  | username            |
| --- | ------------------- |
| 5   | Aniya_Hackett       |
| 7   | Kasandra_Homenick   |
| 14  | Jaclyn81            |
| 21  | Rocio33             |
| 24  | Maxwell.Halvorson   |
| 25  | Tierra.Trantow      |
| 34  | Pearl7              |
| 36  | Ollie_Ledner37      |
| 41  | Mckenna17           |
| 45  | David.Osinski47     |
| 49  | Morgan.Kassulke     |
| 53  | Linnea59            |
| 54  | Duane60             |
| 57  | Julien_Schmidt      |
| 66  | Mike.Auer39         |
| 68  | Franco_Keebler64    |
| 71  | Nia_Haag            |
| 74  | Hulda.Macejkovic    |
| 75  | Leslie67            |
| 76  | Janelle.Nikolaus81  |
| 80  | Darby_Herzog        |
| 81  | Esther.Zulauf61     |
| 83  | Bartholome.Bernhard |
| 89  | Jessyca_West        |
| 90  | Esmeralda.Mraz57    |
| 91  | Bethany20           |

### 3. Declaring Contest Winner
---
The team started a contest and the user who gets the most likes on a single photo will win the contest now they wish to declare the winner.

> **Task**:  Identify the winner of the contest and provide their details to the team

> **Approach**: 

```
select * from users u inner join photos p on u.id = p.user_id  where p.id = (select top(1) photo_id  from likes group by photo_id order by count(photo_id) desc) ;
```
> This query selects the user details from the users table. It then uses a subquery to retrieve the photo_id values from the likes table. The main query filters out the users whose photo_id values is equal to subquery result, indicating top 1 user as winner

```
select top(1) users.id as user_id, users.username, photos.id as photo_id, photos.image_url, count(*) as total from photos inner join likes on likes.photo_id = photos.id inner join users on photos.user_id = users.id group by users.id , users.username, photos.id , photos.image_url order by total DESC;
```

>This query joins the users, photos, and likes tables to retrieve the username, image URL, and the count of likes for each user's photo. It then groups the results by the username and image URL. Finally, it sorts the results in descending order based on the like count and limits the result to only one row, which represents the user with the most likes on a single photo.

> **Results**: Given below is winner of the contest with detail

| user_id | username      | photo_id | image_url           | total |
| ------- | ------------- | -------- | ------------------- | ----- |
| 52      | Zack_Kemmer93 | 145      | https://jarret.name | 48    |


<br>

### 4. Hashtag Researching
---
A partner brand wants to know, which hashtags to use in the post to reach the most people on the platform. 

> **Task**: Identify and suggest the top 5 most commonly used hashtags on the platform

> **Approach**: 

```
select * from tags where id in (select top(5) tag_id from photo_tags group by tag_id order by count(photo_id) desc); 
```
> This query uses the subquery result to get top 5 rows with the highest tag count from photo_tags table. The main query filters out the tags whose id values are present in the subquery result, indicating that they are most used tags.

```
SELECT top(5) t.tag_name, COUNT(*) AS tag_count FROM tags t JOIN photo_tags pt ON t.id = pt.tag_id GROUP BY t.tag_name ORDER BY tag_count DESC;
```

> This query directly uses the TOP(5) clause to limit the result to the top 5 rows with the highest tag count. It performs the necessary join between the tags and photo_tags tables and groups the data by the tag_name column.

> **Results**: Given below are top 5 most commonly used hashtags on the platform

| tag_name | tag_count |
| -------- | --------- |
| smile    | 59        |
| beach    | 42        |
| party    | 39        |
| fun      | 38        |
| food     | 24        |

<br>


### 5. Launch AD Campaign:
---
The team wants to know, which day would be the best day to launch ADs.

> **Task**: What day of the week do most users register on? Provide insights on when to schedule an ad campaign

> **Approach**: 

```
SELECT top(1)
    datename(WEEKDAY, created_at) AS registration_day,
    COUNT(*) AS user_count FROM users
GROUP BY 
    datename(WEEKDAY, created_at)
ORDER BY 
    user_count DESC;
```

> **Results**: Given below are the day of the week which most users are registered

> **Insights**: As per my analysis ad campaign should be scheduled on Sunday as 16 users registered on that day.

| registration_day | user_count |
| ---------------- | ---------- |
| Sunday           | 16         |

<br>

## B) Investor Metrics: 
Our investors want to know if Instagram is performing well and is not becoming redundant like Facebook, they want to assess the app on the following grounds 

### 1. User Engagement:
---
Are users still as active and post on Instagram or they are making fewer posts

> **Task**: Provide how many times does average user posts on Instagram. Also, provide the total number of photos on Instagram/total number of users

> **Approach**: 

```
SELECT COUNT(*)/COUNT(DISTINCT user_id) AS avg_posts_per_user, COUNT(*) AS total_photos, COUNT(DISTINCT user_id) AS total_users from photos
```

> **Results**: Given below is average user posts on Instagram 

| avg_posts_per_user | total_photos | total_users |
| ------------------ | ------------ | ----------- |
| 3.4730             | 257          | 74          |

<br>


### 2. Bots & Fake Accounts:
---
The investors want to know if the platform is crowded with fake and dummy accounts

> **Task**: Provide data on users (bots) who have liked every single photo on the site (since any normal user would not be able to do this).

> **Approach**: 

```
select u.id Users_id, username from users u inner join likes l
on u.id= l.user_id
group by id, username 
having count(*) = (select count(*) from photos);
```

> **Results**: Given below are users (bots) who have liked every single photo on the site 

| Users_id | username           |
| -------- | ------------------ |
| 5        | Aniya_Hackett      |
| 14       | Jaclyn81           |
| 21       | Rocio33            |
| 24       | Maxwell.Halvorson  |
| 36       | Ollie_Ledner37     |
| 41       | Mckenna17          |
| 54       | Duane60            |
| 57       | Julien_Schmidt     |
| 66       | Mike.Auer39        |
| 71       | Nia_Haag           |
| 75       | Leslie67           |
| 76       | Janelle.Nikolaus81 |
| 91       | Bethany20          |

<br>

