## Add custom cell into UICollectionViewCell pitfalls!

#### 1. UILabel not shown multiple lines when slide from multiple-line to single-line.
- You have to set UILabel's `preferredMaxLayoutWidth`: it's the width at which the label will stop growing horizontally and start growing vertically.
- Code:  
     `[hotDetailView.descLabel setPreferredMaxLayoutWidth: CGRectGetWidth(self.mainCollectionView.frame)];`


#### 2. Multiple cell overlap.

- When dequeuewithidentifier, the result cell may already contains old content, especially you add a loose view with tag in it. so firstly you have to remove the added view from cell, and then add the new cell in it.

        cell = [collectionView dequeueReusableCellWithReuseIdentifier:hotDetialIdentifier forIndexPath:indexPath];
        
        if (cell != nil) {
            // remove old detailView first
            hotDetailView = (RDPVoiceDetailView *)[cell viewWithTag:1];
            if (hotDetailView != nil) {
                [hotDetailView removeFromSuperview];
            }
        }
        

### 3. Normal cell, loading cell and endCell have different identifier!

    [self.mainCollectionView registerClass:[UICollectionViewCell class] forCellWithReuseIdentifier:hotDetialIdentifier];
    [self.mainCollectionView registerClass:[UICollectionViewCell class] forCellWithReuseIdentifier:loadingIdentifier];
    [self.mainCollectionView registerClass:[UICollectionViewCell class] forCellWithReuseIdentifier:endIdentifier];
    



    if (index < _voiceSource.currentCount) {
        cell = [collectionView dequeueReusableCellWithReuseIdentifier:hotDetialIdentifier forIndexPath:indexPath];
        
        if (cell != nil) {
            // remove old detailView first
            hotDetailView = (RDPVoiceDetailView *)[cell viewWithTag:1];
            if (hotDetailView != nil) {
                [hotDetailView removeFromSuperview];
            }
        }
        
        // Load RDPHotView
        NSArray* nibViews = [[NSBundle mainBundle] loadNibNamed:@"RDPVoiceDetailView"
                                                          owner:nil
                                                        options:nil];
        
        
        hotDetailView = (RDPVoiceDetailView*)[nibViews objectAtIndex:0];
        hotDetailView.tag = 1;
        // Data
        RDPHotModel *model = [_voiceSource.dataSource objectAtIndex:index];
        [hotDetailView.photoView sd_setImageWithURL:[NSURL URLWithString:model.imagePath]];
        [hotDetailView.descLabel setText:model.descText];
        [hotDetailView.descLabel setPreferredMaxLayoutWidth: CGRectGetWidth(self.mainCollectionView.frame)];
        [hotDetailView setVoiceName:model.voicePath];
        [hotDetailView setVid:model.voice_id];
        [hotDetailView.username setText:model.nickname];
        [hotDetailView.locationButton setHidden:YES];
        [hotDetailView.distance setHidden:YES];
        // Add tag to detailView's play button
        [hotDetailView.playButton setTag:index];
        [hotDetailView.playButton addTarget:self
                                     action:@selector(clickedPlayButton:)
                           forControlEvents:UIControlEventTouchUpInside];
        
        [cell addSubview: hotDetailView];
        // Constraints
        [hotDetailView autoPinEdgeToSuperviewEdge:ALEdgeTop];
        [hotDetailView autoPinEdgeToSuperviewEdge:ALEdgeBottom];
        [hotDetailView autoPinEdgeToSuperviewEdge:ALEdgeLeading];
        [hotDetailView autoPinEdgeToSuperviewEdge:ALEdgeTrailing];
    } else if(index < _voiceSource.totalCount) {
        // empty page
        cell = [collectionView dequeueReusableCellWithReuseIdentifier:loadingIdentifier forIndexPath:indexPath];
    } else {
        cell = [collectionView dequeueReusableCellWithReuseIdentifier:endIdentifier forIndexPath:indexPath];
        
        // Load RDPHotView
        NSArray* nibViews = [[NSBundle mainBundle] loadNibNamed:@"RDPEndRecordView"
                                                          owner:nil
                                                        options:nil];
        
        RDPEndRecordView *endView = (RDPEndRecordView *)[nibViews objectAtIndex:0];
        [cell addSubview:endView];
        
        // Constraints
        [endView autoPinEdgeToSuperviewEdge:ALEdgeTop];
        [endView autoPinEdgeToSuperviewEdge:ALEdgeBottom];
        [endView autoPinEdgeToSuperviewEdge:ALEdgeLeading];
        [endView autoPinEdgeToSuperviewEdge:ALEdgeTrailing];
    }
    
    return cell;