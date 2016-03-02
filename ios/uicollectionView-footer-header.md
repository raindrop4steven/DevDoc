## UICollectionView Header & Footer

	- (UICollectionReusableView *)collectionView:(UICollectionView *)collectionView viewForSupplementaryElementOfKind:(NSString *)kind atIndexPath:(NSIndexPath *)indexPath
	{
	    UICollectionReusableView *reusableview = nil;
	    
	    if (kind == UICollectionElementKindSectionHeader) {
	        RecipeCollectionHeaderView *headerView = [collectionView dequeueReusableSupplementaryViewOfKind:UICollectionElementKindSectionHeader withReuseIdentifier:@"HeaderView" forIndexPath:indexPath];
	        NSString *title = [[NSString alloc]initWithFormat:@"Recipe Group #%i", indexPath.section + 1];
	        headerView.title.text = title;
	        UIImage *headerImage = [UIImage imageNamed:@"header_banner.png"];
	        headerView.backgroundImage.image = headerImage;
	        
	        reusableview = headerView;
	    }
	 
	    if (kind == UICollectionElementKindSectionFooter) {
	        UICollectionReusableView *footerview = [collectionView dequeueReusableSupplementaryViewOfKind:UICollectionElementKindSectionFooter withReuseIdentifier:@"FooterView" forIndexPath:indexPath];
	 
	        reusableview = footerview;
	    }
	        
	    return reusableview;
	}