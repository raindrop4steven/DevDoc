## Url for 

	return jsonify({'nickname': user.nickname, 'avatar': url_for('static', filename=user.avatar, _external=True)})

	{
	  "avatar": "http://localhost:5000/static/avatar-364.jpg",
	  "nickname": "sfsafasfasf"
	}
