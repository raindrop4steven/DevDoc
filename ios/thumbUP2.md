## 评分策略实现

1. 用户点击+/-，按钮disabled,将对应的cell的id存储进本地的数据库，不在缓存中。同时将数据发送到后台，返回最新的得分，刷新得分。

		- (void)clickScore:(ScoreType)type AtIndex:(NSInteger)index
		{
			// Query local db if already tapped
			if already_prized 
			{
				alert("Already prized");
			} else {
				self.score += -1/1;
				distableButton();
				if(ScoreType == plus)
					score = 1
				else
					score = -1
				sendToServer(score) success {
					self.score = score_from_server;
				} failed {
					rolled_back();
				}
			}
		}