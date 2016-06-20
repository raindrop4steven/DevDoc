## only set height for certain row

    - (CGFloat)tableView:(UITableView *)tableView heightForRowAtIndexPath:(NSIndexPath *)indexPath {
        if (indexPath.row == 0) {
            return self.mainCollectionView.frame.size.width;
        }else if(indexPath.row == 1){
            return tableView.rowHeight; // default row height
        }
        
        return 0;
    }