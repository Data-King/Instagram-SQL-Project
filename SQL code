-- Users

CREATE TABLE users(
    id INTEGER UNIQUE PRIMARY KEY AUTOINCREMENT,
    username VARCHAR(255) NOT NULL,
    created_at TIMESTAMP DEFAULT now
);

-- Photos
CREATE TABLE photos(
    id INTEGER AUTO_INCREMENT PRIMARY KEY,
    image_url VARCHAR(355) NOT NULL,
    user_id INT NOT NULL,
    created_date TIMESTAMP DEFAULT now,
    Foreign Key (user_id) REFERENCES users(id)
);

-- Comments 
CREATE TABLE comments(
    id INTEGERAUTOINCREMENT PRIMARY KEY,
    comment_text VARCHAR(255) NOT NULL,
    user_id INT NOT NULL,
    photo_id INT NOT NULL,
    created_at TIMESTAMP DEFAULT now,
    Foreign Key (user_id) REFERENCES users(id),
    Foreign Key (photo_id) REFERENCES photo(id)
);


-- Likes
CREATE TABLE likes(
    user_id INT NOT NULL,
    photo_id INT NOT NULL,
    created_at TIMESTAMP DEFAULT now,
    Foreign Key (user_id) REFERENCES users(id),
    Foreign Key (photo_id) REFERENCES photo(id),
    PRIMARY KEY(user_id,photo_id)
);


-- Follows
CREATE TABLE follows(
	follower_id INT NOT NULL,
	followee_id INT NOT NULL,
	created_at TIMESTAMP DEFAULT now,
	FOREIGN KEY (follower_id) REFERENCES users(id),
	FOREIGN KEY (followee_id) REFERENCES users(id),
	PRIMARY KEY(follower_id,followee_id)
);





-- Tags
CREATE TABLE tags(
	id INTEGER AUTO_INCREMENT PRIMARY KEY,
	tag_name VARCHAR(255) UNIQUE NOT NULL,
	created_at TIMESTAMP DEFAULT now
);


-- Junction table: Photos - Tags
CREATE TABLE photo_tags(
	photo_id INT NOT NULL,
	tag_id INT NOT NULL,
	FOREIGN KEY(photo_id) REFERENCES photos(id),
	FOREIGN KEY(tag_id) REFERENCES tags(id),
	PRIMARY KEY(photo_id,tag_id)
);

-- Data was inserted already



-- We want to reward our users who have been around the longest 
-- Find the 10 oldest users

SELECT * FROM users
ORDER BY created_at
LIMIT 5;



-- What day of the week do most users register on?
-- We need to figure out when to schedule an ad company


SELECT
    date(created_at) as day,
    count(*) as total
FROM users
GROUP BY day
ORDER BY total DESC
LIMIT 2;




-- We want to target our inactive users with an email campaign

SELECT username
FROM users
LEFT JOIN photos on user_id = photos.user_id
WHERE photos.id IS NULL;


-- We are running a new contest to see wgo can get the most likes on a single photo

SELECT
username,
photos.id,
photos.image_url,
count(*) AS total
FROM photos
INNER JOIN likes
     ON likes.photo_id = photo_id
INNER JOIN users
    ON photos.user_id 
GROUP BY photo_id
ORDER BY total DESC
LIMIT 1;



-- Our investors want to know...
-- How many times does the average user post?
-- Total number of photos/total number of users

SELECT round((SELECT count(*) FROM photos)/ (SELECT count(*) FROM users),2);

-- User tanking by postings higher to lower
SELECT users.username, count(photos.image_url)
FROM users
JOIN photos ON user_id = photos.id
GROUP BY user_id
ORDER BY 2 DESC;


-- Total Posts by users

SELECT sum(user_posts.total_posts_per_user)
FROM (SELECT users.username, count(photos.image_url)) AS total_posts_per_user
               FROM users
               JOIN photos ON user_id = photos.user_id
               GROUP BY user_id AS user_posts;




-- Total number of users who gave posted at least one time 

SELECT count(DISTINCT(users.id)) AS total_number_of_users_with_posts
FROM users
JOIN photos ON user_id = photos.user_id;


-- A brand wants to know which hastags to use in a post
-- What are the top 5 most commonly used hashtags
SELECT tag_name, count(tag_name) AS total
FROM tags
JOIN photo_tags ON tag_id = photo_tags.tag_id
GROUP BY tag_id
ORDER BY total DESC;



-- We have a small problems with bots on our site
-- Find users who have liked every single photo on the site

SELECT users.id, username, count(users.id) AS total_likes_by_user
FROM users
JOIN likes ON user_id = likes.user_id
GROUP BY user_id
HAVING total_likes_by_user = (SELECT count(*) FROM photos);


-- We have a bot problem with celebrities
-- Find users who have never commented on a photo
SELECT username, comment_text
FROM users
LEFT JOIN comments ON user_id = comments.user_id
GROUP BY user_id
HAVING comment_text IS NULL;



-- Verison 2

SELECT count(*) 
FROM
(SELECT username, comment_text
FROM users
LEFT JOIN comments ON user_id = comments.user_id
GROUP BY user_id
HAVING comment_text ISNULL) AS total_number_of_users_without_comments;
