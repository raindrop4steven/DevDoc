## iOS Comment

1. [iOS TTTAttributedLabel](https://github.com/TTTAttributedLabel/TTTAttributedLabel)



### 思路：
1. Autopurelayout 遍历评论

		UIScrollView
		Content Size

2. SQL
	
		-- Create table --
		CREATE TABLE Comments(
			id int primary_key,
			voice_id int foreign_key(voices.id)
			session_id int unique not null,
			from_user int not null foreign_key(users.id),
			to_user int not null foreign_key(user.id),
			content String(80) not null,
			time datetime not null
		);

		-- Select Comment -- 
		SELECT * FROM
			comments
		WHERE
			voice_id = #voice_id#
		GROUP BY
			session_id
		ORDER BY
			time ASC
		

### Links
1. [ios-like](./ios-like.md)
2. [ios-ui](./iosUIGithub.md)