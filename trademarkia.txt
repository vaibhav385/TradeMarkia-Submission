CREATE TABLE users
(
    id INTEGER AUTO_INCREMENT PRIMARY KEY,
    username VARCHAR(255) UNIQUE NOT NULL,
    date_of_birth DATE
);
CREATE TABLE posts
(
    id INTEGER AUTO_INCREMENT PRIMARY KEY,
    username VARCHAR(255) UNIQUE NOT NULL,
    title VARCHAR(50) NOT NULL,
    about VARCHAR(255)NOT NULL,
    created_at TIMESTAMP DEFAULT NOW(),
    user_id INTEGER NOT NULL,
    FOREIGN KEY(user_id) REFERENCES users(id)
);
CREATE TABLE comments (
    id INTEGER AUTO_INCREMENT PRIMARY KEY,
    comment_text VARCHAR(255) NOT NULL,
    score INT,
    post_id INTEGER NOT NULL,
    created_at TIMESTAMP DEFAULT NOW(),
    FOREIGN KEY(post_id) REFERENCES posts(id)
);
CREATE TABLE nested_comments (
    id INTEGER AUTO_INCREMENT PRIMARY KEY,
    nested_comment_text VARCHAR(255) NOT NULL,  
    comment_id INTEGER NOT NULL,
    created_at TIMESTAMP DEFAULT NOW(),
    FOREIGN KEY(comment_id) REFERENCES comments(id)
);


--  fetch the post along with its poster and all comments

SELECT username,title,about,comment_text=(SELECT comment_text FROM comments ORDER BY score*score AND created_at DESC) FROM posts,comments WHERE posts.id=comments.post_id;